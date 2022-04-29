---
date: 2022-04-27T22:48:54
title: "EDITOR variable as a script"
subtitle: When you want to automate an interactive session
tags:
  - Developer
categories: [Developer, got ya]
---

I've decided I'm going to start writing again, even small things.

This week I was trying to create a Kubernetes cluster using [Kops](https://kops.sigs.k8s.io/). In the end it didn't work (mainly because it uses a Classic LoadBalancer and we don't have public IPs in that VPC)

But while I was working with it, I was making use of `kops edit` and that opened a new editor (vim in my case) so that I can tweak the configuration (there were no arguments for what I was trying to do).

Doing the same thing all over again is tedious, so I decided to automate it. It turns out `kops edit` triggers the content of the variable EDITOR. So if EDITOR is vim, it will trigger vim, if it nano, it will trigger nano. If it is a shell script that automates the change...

So yes, I created a script, and low and behold, the script was triggered.

Two small caveats, though:

* The script needs to have execution permissions (chmod 755)
* The script needs to have the usual shebang (i.e. it needs to start with `#!/bin/bash` or whatever)

And that's it.

