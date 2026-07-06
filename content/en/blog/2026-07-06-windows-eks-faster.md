---
date: 2026-07-06T20:30:00
title: Some tips for Windows EKS node startup time
subtitle: Half startup time
tags:
  - Developer
categories: [Developer]
toc: false
draft: false
---

## Some tips for Windows EKS node startup time

As I've mentioned before, we use [Windows nodes]({{< relref "2022-10-14-windows-2022-eks" >}}) (and Linux as well) in our build cluster.

A Linux EC2 instance might take 2 minutes from start until it joins an EKS Kubernetes cluster. A similar instance but with Windows will take more than 5 minutes at the very least.

On top of that, Windows containers are humongous. Really big. As in +50GB big. In our case 70GB

So your regular Windows container that needs a new Windows node might take those 5.5 minutes plus another 20-30 minutes to pull the image.

The traditional recommendation is basically to build a personalized [AMI with images cached](https://aws.amazon.com/blogs/containers/speeding-up-windows-container-launch-times-with-ec2-image-builder-and-image-cache-strategy/)

That is what we've been doing. A 300GB AMI in our case. With a startup time of 12-15 minutes.

Not kidding.

We have a very dynamic cluster (we use Karpenter). But we keep Windows empty nodes around for 30 minutes to avoid that penalty.

Yes, you can use [Fast launch to have faster launches](https://aws.amazon.com/about-aws/whats-new/2022/01/aws-speed-optimizations-windows-instances-ec2/). But that is expensive. Quite expensive.

More recently I came up with an idea. After we solved our [predicament with Windows 1.34 nodes]({{< relref "2026-07-05-powershell-containerd-2" >}}), I've been testing it.

Today the results came back.

__From 12 minutes startup time to 6 minutes startup time__

I halved the startup time!!!

It is a bit slower than a regular Windows node. But it comes with the images cached.

How did I do it?

I first created a snapshot of a disk

1. Create a new AMI with two disks.
2. Change container_root in the containerd config to point to a folder in the second disk (say d:\container_root)
3. Pull the images that I need

Then, on Karpenter, create a disk from that snapshot, using the regular Windows AMI. At boot time

1. Mount the disk on D.
2. Change again container_root to point to the folder in D (again d:\container_root)

And that's it.

I guess the speed up comes from two places:
1. Faster to initialize 1 disk of 50GB + 1 disk of 150GB than a single disk of 300GB
2. The AMI is the Amazon one.
