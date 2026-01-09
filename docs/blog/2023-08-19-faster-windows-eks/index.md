---title: Improving Windows build times in EKS
date: 2023-08-19T09:30:00Z
subtitle: Some recipes for faster windows builds in AWS
categories: [Developer]
tags: [Developer got ya]
toc: true
---
## Improving Windows build times in EKS

I cannot believe a year has passed since my last annotation ([which was also EKS and Windows related](/blog/2022-10-14-windows-2022-eks)).

One of our most recent projects has been moving most of our build infrastructure to the cloud. The move has been great, with people waiting less for builds, decomissioning crazy big machines, while keeping an eye on cloud costs, and doing it using IaC.

However, nailing down the Windows pipelines have been more complicated and I wanted to share a few recipes.

### Cache images

Windows container images tend to be humongous. If you're building your own AMIs, [you may want to pull the container images you tend to use](https://gonzalo.f-v.es/blog/2022-10-14-windows-2022-eks/#pull-essential-images). In that way pods will start faster.

Everybody knows about it and [AWS recommends the approach](https://aws.amazon.com/blogs/containers/speeding-up-windows-container-launch-times-with-ec2-image-builder-and-image-cache-strategy/). But in case you missed the memo.

### Slow cloning times

We were seeing slow cloning times. Network performance was fine, disk performance was fine. No clear culprit or wrong metric.

But `git checkout` performance was slow.

Some recipes.

#### Antivirus

I'm not sure what triggered the mental process. Some weird metric. But I remember thinking `What if the issue is antivirus running while the code is being downloaded?`

So we tried to tell Windows defender to skip checking. In the AMI

```powershell
powershell -inputformat none -outputformat none add-mppreference -ExclusionPath 'c:/'
```

and boy, we did noticed such an improvement!!!

Of course you can change it to skip a different path (`c:/var/lib/kubelet/` instead of `c:\` is a good idea)

#### Proper resources for agents

This is not specific for Windows builds (in fact we noticed it a Linux based Jenkins build).

We had defined resources for the agent (the sidecar that runs with the main container as part of the build pod). And we were noticing again low checkout speeds.

In this case, the agent container was being throttled.

Expanding the resources was enough to improve the performance.

### EBS initialization

We were still seeing some performance penalty. Mostly related to cloning times, but not only.

There is a penalty the first time you access an EBS volume that comes from a snapshot. And EBS volumes that come from AMIs will have that penalty.

We tried the [proposed solution (using FIO)](https://docs.aws.amazon.com/AWSEC2/latest/WindowsGuide/ebs-initialize.html) on the user-data, but the nodes wouldn't join the cluster (unclear why).

So we decided to use ephemeral instance storage.

#### Windows instance storage

If you want to use ephemeral storage, you first need to start by using instance types that have ephemeral storage. In our case we went for `m6id.4xlarge` and similar.

Then the launch template has to use two block device mappings: one for the AMI that uses EBS and a second (or more) for mounting the ephemeral disk. Not using EBS for the AMI is a PITA ([I'm not even sure it is possible](https://docs.aws.amazon.com/AWSEC2/latest/UserGuide/creating-an-ami-instance-store.html)).

And finally, you need to use the ephemeral storage. Basically you format the disk, you assign to one new letter/drive, and you tell Kubelet to use that one letter (using [root-dir](https://kubernetes.io/docs/reference/command-line-tools-reference/kubelet/#:~:text=root%2ddir))

```powershell

$contentToAdd = @"
select disk 1
convert dynamic
create volume simple disk 1
select volume 1
assign letter=D
format quick
"@

#Format the disk and add a new drive
Add-Content "C:\Users\Administrator\diskpart.cmd" $contentToAdd
diskpart /s C:\Users\Administrator\diskpart.cmd

#Join the cluster
[string]$EKSBinDir = "$env:ProgramFiles\Amazon\EKS"
[string]$EKSBootstrapScriptName = "Start-EKSBootstrap.ps1"
#This is the key part. Tell kubelet to use the new disk
[string]$KLExtraArgs = '--root-dir=d:/'
[string]$EKSBootstrapScriptFile = "$EKSBinDir\$EKSBootstrapScriptName"
& $EKSBootstrapScriptFile -EKSClusterName <clustername> -ContainerRuntime containerd -KubeletExtraArgs $KLExtraArgs 3>&1 4>&1 5>&1 6>&1
```

#### Windows empty EBS

Using ephemeral storage worked. And it worked well.

But instance types with ephemeral storage are more expensive. It is questionable if that the case if you include EBS costs at well, but your EBS disk might be smaller. But if you use spot instances, you have a bunch more of instance types to choose from.

Also, passing `--root-dir` to kubelet seems to be a [no-go for Karpenter any time soon](https://github.com/aws/karpenter/issues/4411).

So I decided to explore using EBS but with an empty EBS volume.

As I mentioned, the initialization penalty only happens when the volume comes from a snapshot. If it is an empty volume, you're fine!!!

So same as before, I would format the new disk and mounted it.

But then I thought `what would happen if I mounted the disk in the original place so that I don't need to change kubelet default path?`.

Yes, you can mount a disk under a specific folder in Windows (and obviously in Linux).

So yes, that can be done.

```powershell

$contentToAdd = @"
select disk 1
convert dynamic
create volume simple disk 1
select volume 1
assign mount="C:\var\lib\kubelet"
format quick
"@

#Format the disk and add a new drive
Add-Content "C:\Users\Administrator\diskpart.cmd" $contentToAdd
diskpart /s C:\Users\Administrator\diskpart.cmd

#Join the cluster
[string]$EKSBinDir = "$env:ProgramFiles\Amazon\EKS"
[string]$EKSBootstrapScriptName = "Start-EKSBootstrap.ps1"
[string]$EKSBootstrapScriptFile = "$EKSBinDir\$EKSBootstrapScriptName"
& $EKSBootstrapScriptFile -EKSClusterName <clustername> -ContainerRuntime containerd 3>&1 4>&1 5>&1 6>&1
```

This solution to not passing root-dir, works for both EBS and ephemeral storage.

### Totally unrelated: one last thing

I cannot say goodbye without posting something else.

If you use the EBS add-on, you might see that the EBS daemonset fails to work on Windows. It turns out it expects a folder to exist so that it can mount it. Other folders are `DirectoryOrCreate` but not this one.

My solution was to use the following

```powershell

new-item -type directory -path "C:\var\lib\kubelet\plugins_registry" -force
```

YMMV

### Conclusion

And that's it. Just a few tips in case they are helpful for somebody else

If you arrive to this page, please let me know if it helped.
