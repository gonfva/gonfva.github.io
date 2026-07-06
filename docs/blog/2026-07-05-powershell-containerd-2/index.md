---title: PowerShell issues in Containerd
date: 2026-07-05T22:30:00Z
subtitle: Good is not perfect
categories: [Developer]
tags: [Developer]
toc: false
---

## PowerShell issues in Containerd v2

***This is not a great post with a great solution. It is just a post for someone who might be desperate and needs some ideas***

In my current company we have [Windows 2022 containers]({{< relref "2022-10-14-windows-2022-eks" >}}).

They tend to work fine.

However, with the upgrade to EKS from 1.33 to 1.34, we started noticing some random build failures. All of the failures contained some message about SetConsoleTitleW in PowerShell (that we were not specifically calling) and the error:

Win32 0xE9 = ERROR_BROKEN_PIPE "No process is on the other end of the pipe"

Since we use personalized AMIs (more on this on a new post soon), every new test started with a 4-hour job to build a new AMI. Since the error only appears randomly, we then required as many as 30 odd jobs to run in order to know if the issue was still present.

In the beginning we only knew that the issue appeared for Windows nodes on the version 1.34. But since our AMIs contain other changes on top of the base Amazon EKS optimized, we were not sure if the problem was elsewhere.

In the end we were able to confirm that the issue was associated with the change from containerd v1 (the version in EKS optimized images 1.33 is 1.7.30) to containerd v2 (the version is 2.1.x).

It seems there were a lot of changes, and some of them have triggered some race conditions. In fact at some point we were convinced that this MR

https://github.com/containerd/containerd/pull/13522

was going to be the solution to our problems.

Unfortunately, it wasn't. We installed containerd 2.3.2. We run our tests. And instead of requiring 10-15 builds for the failure to appear, it required like 25-30. But the error ended up appearing as well.

So progress, but not enough.

We still needed to upgrade the nodes to 1.34, because eventually we want to upgrade to 1.35 and so on.

We noticed the [following text](https://docs.aws.amazon.com/eks/latest/userguide/kubernetes-versions-standard.html#kubernetes-1-35)


    Containerd 1.x End of Support: Kubernetes 1.35 is the last release supporting containerd 1.x. You must switch to containerd 2.0 or later before upgrading to the next Kubernetes version.

OK. So for 1.34 and 1.35 we could still use containerd v1!!!

That was eye opening. And that's what we did

```
write-output "Updating containerd"
stop-service containerd
$version = "1.7.30"
$arch = "amd64"
curl.exe -L0 https://github.com/containerd/containerd/releases/download/v$version/containerd-$version-windows-$arch.tar.gz
tar.exe xvf ./containerd-$version-windows-$arch.tar.gz
Copy-Item -Path .\bin\* -Destination $env:ProgramFiles\containerd\bin\ -recurse -force
Copy-Item -Path c:\windows\temp\config.toml -Destination $env:ProgramFiles\containerd\config.toml -force

restart-service containerd
ctr version
```

(our Packer HCL uploads config.toml beforehand)

This is not the perfect solution. But it gives us some time. We now have Windows nodes in EKS 1.34. We can upgrade to EKS 1.35, including nodes.

And monitor containerd to see if some other bug fix lands.
