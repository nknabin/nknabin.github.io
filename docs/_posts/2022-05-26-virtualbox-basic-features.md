---
layout: post
title: "Virtualbox: Introduction to basic features"
date: 2022-05-26 22:49:11 +0545
categories: virtualization
---

Understanding the features of virtualbox will help you manage your guest OS efficiently according to your use-case and will make troubleshooting a lot easier. Here are some of the features that you will want to know before you become a power user of VirtualBox.

Note that this does not cover all the features but only those that the users need to interact with. Everything else will work out of the box as they are and should not be changed unless you know what you are doing.

### System configuration and display

The first process of building a computer is to set up the motherboard. In virtual machines, all that's need to be done is to allocate processor and primary memory (RAM) that your virtual machine will use.

To access this features, go to Settings. Go to System tab. The following explains the features:

- Base Memory: The maximum amount of RAM a virtual machine can use.
- Boot Order: The boot sequence (similar to the one found in the bios of your host machine).
- Enable EFI Mode: When checked, the guest OS is booted with UEFI and not bios.

Now, go to Processor tab. Here are the basic features:

- Processors: Number of processors available to the guest OS. (Multiple processors can be assigned to a virtual machine. However, if you assign more than your host's capability, VirtualBox will notify you on the bottom bar with Invalid Settings Detected.)

### Storage

The next process is to define the storage for your virtual machine. Go to Storage tab in the Settings. These are two types of connectors that connect storage mediums like a CD or a DVD and a hard drive to a virtual machine.

- IDE controller<br>
Use this for CDs and DVDs. This is usually used for attaching image (iso) files in order to install the guest OS and guest-additions on the guest.

- SATA controller<br>
Use this for your hard drive. When you create a virtual machine, one hard drive is already attached to your SATA controller. Multiple hard drives can be attached to your virtual machine (just like multiple hard drives in a physical machine).

While creating a guest OS, a new hard drive must be created for this machine or an existing one must be attached. When creating a new hard drive, it's usually wise to create a dynamically allocated VDI. Its size can be readjusted later as required. Also, it does not take up all the defined storage but only the size that its actual content requires. This makes it suitable for working with multiple virtual machines.

To manage drives, go to File > Virtual Media Manager or press Ctrl + D. It can be used to manage hard drives and optical disks that are (or not) attached to any guest OS, i.e. create a new one, add an existing one to a guest or release an attached drive from a guest.

### Network

Go to Network tab on the Settings. There are four adapters available for use. By default, only one is enabled. These are the basic network modes that you will need to understand:

- NAT (Network Address Translation)<br>
This is the default mode for every new guest and will work fine for almost all cases. Every client will get 10.0.2.15 IP address since every guest is an isolated machine of its own. No two machines on NAT can talk to each other. The gateway for every guest will be 10.0.2.2. Every request from the guest OS will be translated to a request from the host.

- Bridged Networking<br>
With this mode, a virtual machine is the equivalent of the host machine on the network. The virtual machine will appear as a separate machine on the network. Each request from the guest OS will be forwarded as its own request. The virtual machine will take one of the IP from your network's IP pool. Hence, this is undesired when your company/client don't want you eating IP addresses.

- Internal Networking<br>
Multiple virtual machines share one internal network so that virtual machines on the same internal network can see each other. Internal networks are isolated among themselves. This is useful for testing when multiple virtual machines are required to be a part of a separate network (for example: testing ftp server/client). However, no DHCP service is provided. Hence, all the members of an internal network must be statically configured or one must serve as a DHCP server.

- Host-Only Networking<br>
It is similar to Internal Networking except the host can provide DHCP service to the members of this network. However, this network is not visible outside the host, hence the name host-only networking. To edit host-only networks, go to File > Host Network Manager or press Ctrl + H.

- Port-Forwarding with NAT<br> If you need to talk to your virtual machine from outside the host (or without the GUI window), then configure the virtual machine to use port-forwarding with NAT.  This will allow multiple machines, even outside your network, to connect to your virtual machine by connecting to your host on the port specified on the Port Forwarding section of NAT.

### Power on/off options

VirtualBox offers you more than one way to start and power down your guest.

- Normal Start<br>
Starts the guest in a normal VirtualBox guest window. This is also the default behavior when a guest OS is started. To close the window, the virtual machine must be closed (powered down) as well.

- Headless Start<br>
Starts the guest without any guest window. This is useful for working with servers. The guest OS takes less memory since it does not require any graphical processing.

- Detachable Start<br>
It is the combination of Normal start and headless start. It starts the guest in a new VirtualBox guest window. However, this new window can be hidden (the guest runs on background), i.e. the window can be closed without shutting down the virtual machine. To show the window again, select the guest machine from the list and click on Show in the toolbar.

Here are the methods to Power down a virtual machine:

- ACPI Shutdown<br>
It sends the shutdown signal to the guest OS, like pressing the power button in a physical machine. The guest OS receives a request to turn off the machine. It is a graceful shutdown method and this should be used to safely turn off a virtual machine.

- Power Off<br>
It is like pulling the plug on a real physical machine. The guest machine is turned off. It should not be used when you have unsaved works.

- Save State<br>
It is equivalent to 'hibernate' behaviour of a physical machine. It saves the current state of your machine and then starts off at the same state the next time. It is the most useful for the same. While in this state, all the memory consumed by the guest is saved and the guest frees the physical machine of the real resources (processor and memory among others).

- Reset<br>
It is rebooting the guest OS. It does not power down the guest in a graceful way before starting it again. It just power offs the guest then start in again.

- Pause<br>
In this state, the guest is paused - it is not using any CPU nor it can start off new processes. However, it will still consume the memory it was consuming before this state was initiated. This is useful when you want to temporarily free the processor in your machine.

### Snapshots

Snapshots are similar to recovery points. The snapshot contains the states of the virtual machine at the time of creation. When restored, the machine is restored to this very state. Multiple snapshots can be created and you can switch between them as required by restoring a snapshot. Going back to a snapshot does not delete other snapshots later than that point.

To create, delete and restore a snapshot, select a virtual machine. Go to Machine > Tools. Check Snapshots. The snapshots tool bar will appear on the below the menu bar for the selected machine.

Snapshots are very useful and can save you from lots of troubles. Create a snapshot of a safe point(unbroken system) in a guest OS so that you can go back to that state if something gets broken inside guest later. Multiple snapshots can be added as a virtual machine is upgraded. However, keep in mind that snapshots may take a big space for storage, so don't spam snapshots but use it only when essential. You can configure each virtual machine to save its snapshots where you want on the file system.

### Conclusion

These are the most basic features that you need to understand before working with virtual machines. Work with these regularly and see how these features fit your purpose.
