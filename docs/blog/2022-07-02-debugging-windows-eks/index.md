---title: Debugging windows nodes in EKS
date: 2022-07-02T13:30:00Z
subtitle: When the node doesn't see the service network
categories: [Developer]
tags: [Developer got ya]
---
## Debugging windows nodes in EKS

After a couple of days dealing with windows nodes in Kubernetes AWS EKS, I thought I would provide a quick write-up of what I've seen. For two reasons

1. In case in a week time I need to remember what I did, I know where to look at.
2. In case somebody faces the same issues and Google offers them this page (BTW, I don't have any analytics on this page, so shout on Twitter or LinkedIn if you found this useful)

Anyway, we have some nodegroups with windows machines. The machines would join the cluster correctly and `kubectl get nodes` would show the nodes in ready status. But it seemed that pods were having some connectivity problem. We were trying to schedule a jenkins slave pod using the Jenkins [Kubernetes plugin](https://plugins.jenkins.io/kubernetes/) but the jnlp container was dying unable to connect to the jenkins service.

So I RDP into the node, and do a curl to the Jenkins service by IP (the service network is a different "ficticious network"). Unable to connect. Obviously, trying to do nslookup using the DNS service for the cluster (CoreDNS) would have the same issue.

Next, I tried to connect the pod directly, not the service. In AWS, pods have a routable IP. Connecting the pod directly would work.

OK. At that point it was obvious the nodes were having some issue trying to connect to the service network. What could that be?

First, I tried to see if the node had joined correctly the cluster. So time to see the logs for the bootstrap process.

```
more C:\ProgramData\Amazon\EC2-Windows\Launch\Log\UserdataExecution.log
```

didn't show anything strange. All seemed to have gone correctly.

Part of the boot process generates a configuration file for CNI (networking). Time to see that

```
more C:\ProgramData\Amazon\EKS\cni\config\vpc-shared-eni.conf

{
    "cniVersion": "0.3.1",
    "name": "vpc",
    "type": "vpc-shared-eni",
    "eniMACAddress": "<macaddress>",
    "eniIPAddresses": ["<CIDR self>"],
    "gatewayIPAddress": "<ip gateway>",
    "vpcCIDRs": [
        "<CIDR vpc>"
    ],
    "serviceCIDR": "<CIDR service>",
     "dns": {
        "nameservers": ["<ip dns>"],
        "search": [
            "{%namespace%}.svc.cluster.local",
            "svc.cluster.local",
            "cluster.local"
        ]
    }
}
```

Again. Everything correct.

Service network. It should be the kube-proxy. How to to debug kube-proxy in Windows? How to see logs in Windows?

It turns out there is no such thing as journalctl or so. You use `get-eventlog`

In particular, this is the command to get a list of the logs in tabular format

```
> get-eventlog eks|more
```

In that list, something caught my eye inmediately, because it was of type error, not information nor warning.

So I laser focussed on that event

```
> get-eventlog EKS |where index -eq 8556|format-list *

    EventID            : 0
    MachineName        : <name machine>
    Data               : {}
    Index              : 8556
    Category           : (0)
    CategoryNumber     : 0
    EntryType          : Error
    Message            : E0624 12:04:06.002944    3084 reflector.go:138]
                        k8s.io/client-go/informers/factory.go:134: Failed
                        to watch *v1.EndpointSlice: failed to list *v1.EndpointSlice:
                        endpointslices.discovery.k8s.io is forbidden: User
                        "system:node:ip-<ip>.ec2.internal" cannot list resource
                        "endpointslices" in API group "discovery.k8s.io" at the cluster scope
    Source             : kube-proxy
    ReplacementStrings : {E0624 12:04:06.002944    3084 reflector.go:138]
                        k8s.io/client-go/informers/factory.go:134: Failed
                        to watch *v1.EndpointSlice      : failed to list *v1.EndpointSlice:
                        endpointslices.discovery.k8s.io is forbidden: User
                        "system:node:ip-<ip>.ec2.internal" cannot list resource
                        "endpointslices" in API group "discovery.k8s.io" at the cluster scope}
    InstanceId         : 0
    TimeGenerated      : 6/24/2022 12:04:06 PM
    TimeWritten        : 6/24/2022 12:04:06 PM
    UserName           :
    Site               :
    Container          :

```

Something about kube-proxy not being able to access a specific endpoint.
`"system:node:ip-<ip>.ec2.internal" cannot list resource "endpointslices" in API group "discovery.k8s.io"`

Hmm. Immediately I knew it was going to be related to aws-auth. Remember that aws-auth is the way EKS gives permissions to nodes (and actual people) to connect to the cluster.

There was some red-herring with a bug on CoreDNS. But I dismissed it quickly.

It had to be aws-auth.


I checked the configuration, and yes, aws-auth had indeed two groups of nodes. Linux nodes, have applied two clusterrolebindings (I thought it was roles, but not). And windows, have a third clusterrolebinding.

```

$ kubectl get cm aws-auth -n kube-system -o yaml
apiVersion: v1
data:
  mapRoles: |
    - rolearn: <arn linux nodes>
      username: system:node:{{EC2PrivateDNSName}}
      groups:
        - system:bootstrappers
        - system:nodes
    - rolearn: <arn windows nodes>
      username: system:node:{{EC2PrivateDNSName}}
      groups:
        - system:bootstrappers
        - system:nodes
        - eks:kube-proxy-windows
```

So far, so good. I checked that the cluster role binding was correct

```
$ kubectl get clusterrolebinding eks:kube-proxy-windows -o yaml
apiVersion: rbac.authorization.k8s.io/v1
kind: ClusterRoleBinding
metadata:
  [...]
  labels:
    eks.amazonaws.com/component: kube-proxy
    k8s-app: kube-proxy
  name: eks:kube-proxy-windows
  [...]
roleRef:
  apiGroup: rbac.authorization.k8s.io
  kind: ClusterRole
  name: system:node-proxier
subjects:
- apiGroup: rbac.authorization.k8s.io
  kind: Group
  name: eks:kube-proxy-windows

```

There we are, `system:node-proxier`. Everything looks correct. Next, check the Cluster role

```
$ kubectl get clusterrole system:node-proxier -o yaml
apiVersion: rbac.authorization.k8s.io/v1
kind: ClusterRole
metadata:
  annotations:
    rbac.authorization.kubernetes.io/autoupdate: "true"
  labels:
    kubernetes.io/bootstrapping: rbac-defaults
  name: system:node-proxier
  [...]
rules:
  [...]
- apiGroups:
  - discovery.k8s.io
  resources:
  - endpointslices
  verbs:
  - list
  - watch
```
and lo and behold, `discovery.k8s.io` apigroups, `endpointslices` resources, and `list`.

Hmm. Everything looks correct.

Rubber duck moment.

The k8s cluster role should have permissions, the cluster role gets applied to nodes that have the AWS role I have passed for windows machines.

But ... our window machine doesn't have permissions?

Let's check the last piece. It cannot be so stupid...

*Does the window machine have the windows role?*

Yes, in my case, the issue was so stupid. *The node had an incorrect AWS role applied*.

I hope that the whole debugging process was more interesting than actually the issue.
