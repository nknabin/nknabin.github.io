---
layout: post
title: "Virtualization: Get started!"
date: 2022-05-23 14:31:32 +0545
categories: virtualization
---

### What is virtualization?

It lets other operating systems run on your host machine. It simulates multiple operating systems (or other software) running simultaneously on one hardware. The software that does this is called a hypervisor. VirtualBox and VMWare are two hypervisors.

The hypervisor runs multiple virtual systems simultaneously. The system/machine that the hypervisor runs on is called a host machine and the virtual machines are called guest machines.

### Is it any useful?

Depending on your purposes, it is highly useful and comes handy in many cases. Here are some of the things I have used virtualization for:

- Practice<br>
Use virtual machines to try and get accustomed to new programs and tools before you actually install it on the host machines. Host OS is always safe no matter what happens to the virtual machines (VMs).

- Testing software/configurations<br>
Testing new configurations and trying new window managers and desktop environments are easy with VMs.

- Web servers<br>
Use different servers in headless mode so you can focus entirely on your web development.

- Back up<br>
With features like snapshots, virtual machines are a good choice for backups. Besides, as long as you have the virtual disk image files, you can start where you left off in the virtual machine even if you switched to a different operating system or a new machine.

- Run Windows on top of my Linux machine or vice-versa<br>
Run native Linux/Windows applications on virtual machines. Do this for learning or for utilizing the efficiency of each OS.

- Browse in complete safety<br>
Browsing inside a VM makes your host OS completely out of harm's way from malware, viruses and attackers. Using a privacy focused OS like Tails OS even adds more to your online privacy and safety.

### Okay, how do I start?

It's actually very simple. All you have to do is follow the next tutorials! However, you do need to know some things before you start (like which hypervisor to choose). We will use a **Type 2 Hypervisor** - that runs on host like other applications (More on that later after we get familiar with running virtual machines).

We will use **VirtualBox** for these tutorials. Why? Because it's free and functional. Most of the handy features like run multiple machines simultaneously, shared folders between host and guests, clipboard sharing between host and guests, drag and drop and USB 3 support can be used out of the box. It also has a simple and easy to use interface that's great for beginners to get familiar with virtualization.

Although there are other options like VMWare Player is free, it does not provide all of these features with the free version. However, feel free to experiment with it all you want. But, if you a beginner or want all of those features mentioned above, VirtualBox is highly recommended.

### When not to use virtualization?

Before you start, there are some things you need to know about when not to use virtualization. Although virtualization offers you the flexibility of running multiple operating systems simultaneously, it comes with great price: the hardware sharing. Unless you have quite a big pocket to invest in powerful hardware, virtualization is just not for running resource intensive applications like games. Although virtualization offers advantages from running multiple machines simultaneously, nothing is as efficient as fast as compared to the physical machine. So, here are some situation where hypervisors don't provide satisfactory performance:

- Limited Capacity<br>
Unless you have a capable machine, virtualization creates more problems to work with both your host and guests. Anything equivalent to Intel i5 processor and 4 GB ram is enough to try out virtualization and play around.

- Resource Intensive applications<br>
Go with alternatives if you need to play games or use resource intensive apps like video editing software or distributed SMP applications. These are more efficient running off of host OS.

- Server virtualization<br>
Unless you now what you are doing, it just adds a layer of complexity. Simple tasks like managing encryption keys gets more complex and virtualization may increase the downtime of servers.

- Others<br>
Some apps are restricted by their license to run on hypervisors. Some apps that have critical time synchronization requirements may not work as expected at all times since hypervisors maintain their own clock.

### Time to play around!

If you decided to learn more, follow these tutorials to learn more about virtualization and using its full potential to your interests.

See [here]({% post_url 2022-05-23-install-virtualbox %}) for instructions on installing VirtualBox.
