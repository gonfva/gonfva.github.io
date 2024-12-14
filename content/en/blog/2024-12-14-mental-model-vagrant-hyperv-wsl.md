---
date: 2024-12-14T10:30:00
title: My mental model for using Vagrant+Hyper-V and WSL
subtitle: Virtual networks in Windows
tags:
  - Developer
categories: [Developer]
toc: true
---

[**Really dull** post about virtualization in Windows]

For the last couple of months, I've been (on and off) working with others in a project to migrate development environments so that we developers can use both virtual machines with Vagrant+Hyper-V AND WSL.

That's a lot of time ~~wasted~~ invested in ~~googling and reading StackOverflow and Microsoft Learn~~ research of a topic that is significantly away from my [company's](https://www.flextrade.com) core proposition.

As always, I'm pretty sure I will not remember any of this in a couple of ~~days~~ months. So I thought it was a good idea dumping some of the things learned along the way in case ~~I~~ someone else needs it.

So here you can find some crude notes on what my mind understands of the topic as of now.

## WSL networking modes

First, let's start with WSL.

WSL allows to have Windows running side by side a pretty standard Linux distribution. WSL acts as a virtual machine inside your Microsoft Windows host but it is very well integrated. For people that prefer a *nix environment but for whatever reasons need to use a Windows machine, it is awesome.

You can even invoke Windows programs from Linux. You'll see it later that you can invoke a powershell script from Linux (and it will run on the windows host)

Anyway, in the end WSL is a virtual machine. And it will need to connect to a network.

There are two basic networking modes in WSL:
+ **Mirrored**: For the network teams that would be something similar to a bridge. Your WSL will generate a network interface that connects directly to the physical network (Wifi or Ethernet or whatever). By connecting to the physical network, it will get a proper IP from your network (DHCP). More importantly, Microsoft has done some magic there and if you're connecting via VPN to some company network, WSL in mirror mode will work out of the box. That's brilliant. Unfortunately, this mode requires access to(including a new IP from) the physical layer. And in some corporate environments that's not good. Additionally, it doesn't communicate with Hyper-V (more on that later).

+ **NAT (Network Address Translation)**: As a kind of overview, WSL generates an internal network with a specific range, but every time a packet of that internal network needs to go outside (either the Internet or VPN) or a packet from the outside world needs to go inside, Windows will take care of doing a change (translation) of address. Externally, your Windows host IP is the only IP visible. This mode tends to work fine as well. However WSL generates a random CIDR for itself, usually in the 172.16.0.0/12 range, and it might clash with other private networks. If it does, WSL NAT won't work at all. The VPN will tell Windows to use the VPN for the range that is also used by WSL so WSL gets isolated.

There are other network modes, but those are the two main ones. And both are good, but both have the gotchas I explained.

## Hyper-V switches

Hyper-V is a virtualization mechanism available in Windows Pro/Windows Server versions. It allows to create virtual machines where you can install (almost) any operating system.

Hyper-V can also be used as a crude replacement for VMWare and ESXi and that kind of virtualization technologies.

We wanted to use [Vagrant](https://www.vagrantup.com/) as the provision mechanism. Vagrant with Hyper-V  is a bit limited, but we needed some scripting/IaC and pure Hyper-V (interacting with the management console) was not an option.

Vagrant needs for the user to be part of the Hyper-V administrators group, but it is a sort of privilege that is not too wide.

Hyper-V, (similar to with WSL) comes with two main modes of connecting to the network.

+ **External** mode. It is again a bridged mode (similar to the **mirrored** mode in WSL I just mentioned). In this mode, the VM will connect to the switch which connects directly to the physical network (and it will get an IP from that network, because usually there will be a DHCP server there). It usually works BUT it doesn't tunnel via VPN. That was a major drawback for us.
+ The **internal** mode is similar to the NAT mode in WSL. As the name mentions, it creates an internal network and it can connect that network to the outside world using NAT. Unfortunately the internal mode in Hyper-V doesn't have a DHCP internal server (as opposed to what it seems to be the case in WSL).

There is a third mode call private, but we can ignore it here.

For that network access, when you create a VM (directly on Hyper-V or through Vagrant), you need to use a virtual switch. There is a **default switch**, which is of type internal AND it has DHCP server BUT you cannot specify the IP range (so it will clash). Alternatively you can create one (or multiple) virtual switches.

In fact, Windows creates Hyper-V switches for WSL, which suggests that the underlying mechanism is always Hyper-V.

## Assigning IPs

WSL would have worked in both modes. But Hyper-V can only work for us in internal mode, because we couldn't make it work inside the VPN and we needed VPN access.

In the end we opted for internal mode in both cases, because of different use cases. However, in both WSL and Hyper-V we had to do some additional preparation to the internal/NAT mode so that the CIDR ranges chosen was not random but something in our control.

### WSL

In WSL we ended up using NAT mode, but we had first to fix the clash in IP addresses. And the way to do it using an [undocumented registry key](https://github.com/microsoft/WSL/discussions/9580#discussioncomment-5510129) that tells WSL which CIDR range it should use when using its NAT mode.

In the registry you will need to go to
`HKLM\SOFTWARE\Microsoft\Windows\CurrentVersion\Lxss`
And create two values.

+ One `NatNetwork` can be something like `10.0.250.0/24`.
+ The other is `NatGatewayIpAddress` and can be something like `10.0.250.1`.

Obviously choose a range that you know it is not used in your internal networks.

To make sure WSL uses NAT mode, you can either enter WSL settings (an app in Windows) or, if you want to do it at laptop provision time (with no interaction), [use .wslconfig](https://learn.microsoft.com/en-us/windows/wsl/wsl-config).

### Hyper-V

For Hyper-V, things are really, really, really more tricky.

Yes. We zero-ed on creating an internal switch and changing the NAT range as well. As an example
```
New-VMSwitch -SwitchName "Internal" -SwitchType Internal
New-NetIPAddress -IPAddress 10.0.251.1 -PrefixLength 24 -InterfaceAlias "vEthernet (NATSwitch)"
New-NetNat -Name "NATNetwork" -InternalIPInterfaceAddressPrefix 10.0.251.0/24
```

However, that approach still has the problem that internal switches in Hyper-V don't have a **DHCP server**. You might try to set a flag called DHCP. I tried, and I lost the NetIPAddress. Or you might try to change the IP address of the default switch that *already* has DHCP. I did it and completely broke my default switch.

Believe me, I've dedicated a non-trivial amount of time to have a DHCP server in that configuration.

I even fixed a [go library](https://github.com/insomniacslk/dhcp/pull/551) while trying to build a [DHCP server](https://github.com/gonfva/dhcp-server) (I might continue working on it).

**I won't say** it is not possible. It might be. But I can tell it is not trivial.

The only solution we've found is to go inside the VM, and set a static IP inside. The static IP needs to be in the range of the Virtual Switch. If you manage to set the IP in this way (by using Hyper-V to login into the machine), and you set the IP, it will work. This [page is a good explanation](https://medium.com/@mdavis332/hyper-v-nat-w-linux-vm-1d245be6ded1) of how to set the IP inside an Ubuntu VM.

### Enter Vagrant in Hyper-V

However, we wanted to use Vagrant. We provision our VMs with Vagrant (to install a number of things automatically).

How can you do it with Vagrant?
+ If you spin it with Vagrant, and you select the **default switch**, you will get a clash in the IP.
+ If you use an **external switch** you cannot tunnel through the VPN.
+ If you use an **internal switch** (that is not the default switch), it won't have an IPv4 address, so Vagrant won't be able to connect.

The solution we implemented comes with two switches: one external and one internal.

+ Once you create the machine and are asked to select the Hyper-v switch, you select the external switch.
+ In the vagrantfile, the first thing that you do is a provisioning script in the VM that sets a specific IP.
+ The second thing that the vagrantfile does is to use a provisioning script IN THE HOST to switch the network to the internal switch. Something like `get-vm <name-vm>|get-vmnetworkadapter|connect-vmnetworkadapter -switchname <name-switch>`
+ And the third thing to do is to (using the reload plugin) restart the VM.

Once restarted, the VM will now have a static IP (no need to use DHCP) AND the internal switch (NAT) and it can keep using the internal switch from then onwards. Including provisioning and beyond (even after restarting the host machine or stopping/starting the VM)

## Now all together

OK. We now have WSL working both internally and externally. And Hyper-V, internally and externally.

However, there is the **final boss**. You know want to communicate from WSL to Hyper-V VMs (and viceversa).


{{< youtube iRgtzZ-mOQo >}}


The solution is to use
```
set-netipinterface -forwarding enabled
```
for all the interfaces you want to connect. Unfortunately, this command seems to require admin privileges.

Even better is to use
```
netsh interface ipv4 set interface $idx forwarding=Enabled
```

which can be used as a normal (non admin) user.

And **that works**.

**Until you reboot**.

Because WSL creates the WSL switch when you first connect to WSL after the reboot. The switch is new, and it doesn't have forwarding enabled.

You're almost there. Now you need to run a powershell script when WSL starts.

+  You can create a link that launches the script and then WSL.
+  Or you can use crontab to run on reboot
+ Or you can use a systemd service inside WSL that triggers the powershell script

```
[Unit]
Description=Enables the forwarding of the network interace

[Service]
Type=oneshot
ExecStart=/mnt/c/windows/system32/windowspowershell/v1.0//powershell.exe =File <path script>

[Install]
WantedBy=multi-user.target
```

There.

**What a trip**


## Conclusion

Believe me. This is much more than I wanted to know about virtualization in Windows.

On the other hand, I know that I've barely scratched the surface. And it was an interesting learning experience.

I really hope it is useful for someone else.
