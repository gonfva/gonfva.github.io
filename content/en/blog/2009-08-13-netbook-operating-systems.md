---
title: "Netbook Operating Systems"
previous: https://gonfva.blogspot.com/2009/08/netbook-operating-systems.html
date: 2009-08-13T22:01:25
categories:
  - Technology
tags:
  - Technology
---

A few months ago I got a new
[Acer Aspire One](http://www.acer.com/aspireone/aspireone_8_9/) A110.
It's an Atom N270 with 512 MB RAM and 8GB SSD hard disk and 8.9'' screen. It
came with a personalized version of a Linux distribution called
[Linpus Lite](http://www.linpus.com/) based in Fedora 8. Since the
first day I've been testing several Netbook OS and I wanted to share my thoughts
about it.

- The original distribution, (as I've said) is a personalized version of
  Linpus Lite. It boots very fast, and works smoothly. However, it's locked, and configuration and installation of new software (or updating of existing)
  is daunting. Fortunately Acer put a [repository with new packages](ftp://ftp.work.acer-euro.com/netbook/aspire_one_110/linux/application/), although it's still not enough.

- The first OS I tested was [Ubuntu Netbook Remix](http://www.canonical.com/projects/ubuntu/unr). It's a distribution based in Ubuntu 9.04. It has some interesting things,
  like a menu/desktop interface more usable in small screen netbooks than
  traditional Ubuntu or Linpus. It also had access to the full Ubuntu
  repository. However, it took twice or three times more time to boot than
  Linpus. And it seemed sluggish.

- Following I tested [Moblin](http://moblin.org/). It boots very
  fast, and works pretty well. The first version I tested was a bit
  unpolished, but it's being updated pretty fast. However has two things that
  I personally don't like. The first one is that Moblin seems to encourage a
  particular way of working. For example it has a specific place for a
  specific twitter client. And the same for the browser. "What if I want
  another browser or if I don't use Twitter?". The second thing is that
  sometimes is difficult to close and application because top menu gets
  unfolded. To sum up it's very fast, but not designed for me.

- I also tried to test [Live Android OS](http://code.google.com/p/live-android/). It
  worked pretty well on a [VirtualBox](http://www.virtualbox.org/) under Windows 7. However I couldn't make it boot on my Acer, and it seems a [known bug](http://code.google.com/p/live-android/issues/detail?id=11). Of course it's an early port to i386 architecture.

- Next I tested [Jolicloud](http://www.jolicloud.com/). It's a new
  Operating System, and tested only under invitation. It seems based in Ubuntu
  Netbook Remix, and has two very interesting concepts. The first one is that
  in the installation process you have to select the model of the netbook.
  That seems to make a personalized version of the operating system perfectly
  adjusted to the model. The second interesting thing is a great catalog of
  applications some of them web applications, some of them client-only. The
  good thing is that for the user is indifferent. Besides those concepts,
  being based in Ubuntu Netbook Remix have the same problems than the
  original. Slow boot and slow operation. Also, the installed web applications
  seem based in Prism, and I don't feel comfortable clicking links there
  because it has to start a new browser.

- Last I tested [Xubuntu](http://www.xubuntu.org/). Xubuntu works
  better than Ubuntu Netbook Remix, but boots slow. Also I installed the
  governor dock applet. If selected powersave, sometimes it went jerky. If
  selected ondemand battery disappeared. For the record Chrome with plugins is
  still unstable in Linux.

- I also installed **Windows XP** in my
  brother-in-law's netbook, the same brand and model. It could run WXP, but
  again it seemed slow.

The bottom line is that I like Moblin for the speed, Ubuntu Netbook Remix
for the software and look and feel, Jolicloud looks promising... but I'll keep
using Linpus, while waiting to [Google Chrome OS](http://googleblog.blogspot.com/2009/07/introducing-google-chrome-os.html).

What do I want? An operating system that ...<

- ... can boot and launch a browser very quick.

- ...tries to save battery. Bonus point if I can remove an USB stick while suspended without crashing.

- ... runs fast

- ... apart from a browser, has already installed (or one-click away) Flash Plugin PDF viewer, and a media player. Bonus point for an Office suite.

Nothing too strange for a Netbook, I bet.
