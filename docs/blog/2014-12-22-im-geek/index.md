---title: I'm a geek
date: 2014-12-22T13:51:21Z
categories: Technology
tags: [Developer]
---


A few weeks ago, I bid and won a [PowerEdge 2650](http://www.dell.com/downloads/global/products/pedge/en/2650_specs.pdf) server for three quids (plus another 6 of renting a car and collecting it).


It´s an old piece of hardware (32 bits double Xeon, 4gb of RAM), bulky as hell (or so does my back says) and noisy as a diesel generator.


When I came home, my daughters said that couldn't be a computer as it didn't have input/output devices like a keyboard or a monitor. I didn't bother to explain a server does not need a screen or a keyboard. Whether that's correct is arguable. They are learning Computer Science at school, and I only interfere when I see a big mistake.


Anyway, bringing home the computer was only part of the project. Hey, I'm a geek, but having a server on the floor of my dining room is not where the fun lies even if my wife allows it. First, I needed to open its case, take a look at the the inside (a bit insane, yes I know).


Then, I needed to install the operating system. I thought that was going to be easy. I install different operating systems all the time. But...


... I usually install using an USB drive and the server would refuse to boot up from the USB drive.


OK. It has CD-reader but ...


... I don´t have a CD-Writer. Who writes CDs anyway?


Already thinking how/where to record a CD, I decided to investigate something called [PXE](http://en.wikipedia.org/wiki/Preboot_Execution_Environment). PXE tends to appear in many BIOS as install from the network. In very general terms, the BIOS is enough to look for a DHCP server that looks for sort of FTP server, that provides a typical file that allows to install an Operating System.


Unfortunately, I only had a Windows machine available (having two daughters in secondary school, Linux has been unfortunately relegated)


But there is an [old post](http://hugi.to/blog/archive/2006/12/23/ubuntu-pxe-install-via-windows) that explain how to install Ubuntu from windows. The only tweak was the needed to get some current image. You need to get the image from this [URL](ftp://archive.ubuntu.com/ubuntu/dists/trusty/main/installer-i386/current/images/netboot/netboot.tar.gz)


One small tweak while installing is something that had already happened while installing a server at work. The screen would go blank with the message "Cannot display this video mode". A quick search, and one of the first results was [what I needed](http://www.jonwitts.co.uk/archives/208).


Once I installed the basic server, I only installed SSH server. The rest of the server was going to be installed using [Ansible](http://www.ansible.com/home). Another topic for another time


I´m a geek.


And, my wife doesn't read my blog and she is a saint.
