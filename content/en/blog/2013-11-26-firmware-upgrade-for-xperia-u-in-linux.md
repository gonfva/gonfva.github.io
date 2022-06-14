---
layout: post
title: "Firmware upgrade for Xperia U in Linux (II)"
previous: https://gonfva.blogspot.com/2013/11/firmware-upgrade-for-xperia-u-in-linux.html
date: 2013-11-26T21:27:19
comments: false
categories: [got ya]
tags:
  - Developer
---

[Long post]


Edited (08th-Jun-2014): Updated the firmware download site.


In my [previous post](https://gonfva.blogspot.com/2013/11/firmware-upgrade-for-xperia-u-in-linux-i.html) I explained that one of the things that made the installing process more difficult was the lack of a general picture of the process. There are lots of pages explaining recipes to install, but I find it difficult to follow recipes if I don't manage to understand the underlying logic.


The following is my visualization of the whole process (this is less than a week of knowledge, so I cannot say I'm an expert: take it with a grain of salt). You can get a [more accurate picture of the Android architecture in other pages](http://www.android-app-market.com/android-architecture.html). And you can [get the process in detail from XDA](http://forum.xda-developers.com/showthread.php?t=2107775).



#### First, components

As far as I understand there are three components in an Android system




+ The bootloader. This is a piece of software that usually loads the system, enables Android recovery image and image upgrades. Sometimes it is called the kernel, so I understand it is the underlying component of the Android
+ The second one is called the system. It would include shared libraries and components. When people says Android is Java, I understand that it is probably related to this set.
+ And the third one is called Gapps. Gapps it is not a component in its own. It's a package with Google applications. For licence reasons, you cannot distribute them attached to other components, so you almost always will find a package with Gapps.</li></ol>


OK. Now to the process. Again. You'll probably void the warranty and you could brick your phone. If you're not comfortable installing your own operating system at your computer, DON'T EVEN BOTHER reading further. If you are reading this, you have Linux at home and that's means that you are more than average Joe, but do it at tour own risk.


#### Second, the modes. Fastboot and the like


"Fastboot??? What the h***ll is that. Gonzalo, you told me this was going to be easy".


I nearly bricked a BQ Maxwell tablet upgrading to the official firmware, so, please, don't think this is easy.


**Fastboot** is a mode in the phone that allows to unlock and flash the bootloader. In the Xperia U you get that mode turning the phone off, then pressing the Volume up button and while pressing, connecting the USB cable to the computer. But you should already know if you <b>had read** the Sony document as I told you. The light in the phone should be blue.


There is also another mode called  **Flash mode**. "Fastboot mode and Flash mode are different? Is there no other F-word?". To enter Flash mode, shutdown the mobile, disconnect the cable from the computer, press the Volume down, connect to the computer and release the volume down. The light in the phone should be green.


  **Recovery mode**. It is a mode that allows to do some powerful things from the phone. To enter that mode, power off the phone, then power on and after you see the SONY logo, press repeatedly volume down button (-) until you see recovery. While in the previous two modes the screen is powered off, in this mode, you can navigate the menus. To navigate in the recovery mode, you use volume up and volume down, and select with the Power button.


You will sometimes read about  **Normal mode**. Normal mode is the phone on as usual, sometimes with the the debug options activated.


#### Updating. One twist.

After reading about components and modes, the first thing to do it would seem to update the bootloader. But...


...you cannot update the kernel directly because to avoid people messing around, kernels tend to be locked down by the manufacturer. Besides, sometimes you buy phones locked by the telco company (so that you get it cheaper, but you cannot change the company).


So before you update the kernel, you need to unlock (or root, as it sometimes called) the bootloader.


#### Unlocking the bootloader

I must confess I'm pleasantly surprised with Sony about the unlocking process. It has a very informative page with a [step-to-step guide](http://unlockbootloader.sonymobile.com/unlocking-boot-loader). Unfortunately, it doesn't work with Linux, but you should read it in detail because it is almost there. You'll need to install Android SDK. You'll need to follow most of the steps.. Also, you will need a code, you're going to get by email from that page. So go read, click next and fill the details with your email and IMEI, because you do really need the code.


When going for Linux, you're going to follow the same steps. BUT. Instead of the steps related to the USB drivers (i.e step 9), you need UDEV rules. UDEV is the way Linux uses to recognize new components, in particular USB devices. You will need the USB ID for your phone version, both in usual mode, in Fastboot mode and in Flash mode.





Back to the UDEV rules, these are the ones I got for Xperia U ST25i.



<script src="https://gist.github.com/gonfva/7633925.js"></script>



But you should get yours using the tool lsusb.


After you have the correct rules (and you have restarted the UDEV with "sudo service udev restart") connected in fastboot mode, you will be able to follow steps in the Sony Page and unlock the bootloader. And then to the next step



#### Flashing the kernel

This part should be easy but it gets a bit tricky. You are going to use a tool called Flashtool. But the tricky part is that Flashtool needs libusb-dev. From what I've been able to understand, that library suffered some fork, so you'll end up asking how the h*ll should you install that library. And if you've got a 64 bits linux version, things are not going to be easier. You may end up screwing the whole system because ia32-libs (don't follow that route).


Good thing about the Internet is that you are not alone. So in [this page](http://askubuntu.com/questions/366336/how-do-i-install-lsusbx-library), you'll get to ease the problematic steps. Basically, compile libusb-dev from source and modify Flashtool.


And then flahsing the kernel should be easy. Flashtool allows lots of things, but I only [flashed the kernel](http://forum.xda-developers.com/showthread.php?t=928343)  with the firmware Flashtool came with. Once connected, I think you can flash either in Fast boot and in flash mode.


#### Flashing the system and Gapps

The last part is very easy. First download the system and Gapps. <strike>If you're going to pick the latest versions, do it from Maclaw. The download is a bit unintuitive (click Sony Xperia U, click CianogenMod in the right, and then download both files). </strike>Edited: If you want Android 4.4.3, see [this post](http://gonfva.blogspot.co.uk/2014/06/android-443-sony-xperia-u.html). [The better place is from XDA](http://wiki.cyanogenmod.org/w/Unofficial_Ports#Sony_Xperia_U). Once downloaded, copy them to the phone in normal mode.


And reboot in Recovery mode. In Recovery mode you should do a backup. Just in case.
<div class="separator" style="clear: both; text-align: center;"><a href="http://3.bp.blogspot.com/-PH1vKJRPxU4/UpUG1kXe9XI/AAAAAAAAAjU/n6xOwR8A6aI/s1600/26112013201.jpg" imageanchor="1" style="clear: left; float: left; margin-bottom: 1em; margin-right: 1em;"><img border="0" src="http://3.bp.blogspot.com/-PH1vKJRPxU4/UpUG1kXe9XI/AAAAAAAAAjU/n6xOwR8A6aI/s200/26112013201.jpg" height="200" width="150" /></a></div>
Then follow the process "Install zip->Choose zip from /storage/sdcard0 and select the folder where you copied the file" to load first the system file. And then do the same process with the Gapps file.


And then wipe data/factory reset, followed by wipe cache partition and followed by "advanced->Wipe dalvik cache"


And then "reboot system now"


#### One last note


One of the things I struggled with was reading about a sdcard "Where is the SDCARD in an Xperia U?".


I understand it is a sort of internal storage. Anyway, there is no socket to put a SDCARD inside, so forget about it.
