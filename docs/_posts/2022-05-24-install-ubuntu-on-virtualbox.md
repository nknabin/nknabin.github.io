---
layout: post
title: "How to install Ubuntu on VirtualBox"
date: 2022-05-24 16:30:12 +0545
categories: virtualization
---

### Before you start

This tutorial assumes that you have VirtualBox installed. An installed extension pack will be a plus for later tutorials (example: making shared folders). If you have not done this, see [here]({% post_url 2022-05-23-install-virtualbox %}) for installing VirtualBox.

### Download Ubuntu Desktop image file (.iso) from Ubuntu.

Make sure you have at least 10 GB free space on your hard drive. This is enough for getting Ubuntu installed and running. However if you intend to work inside VirtualBox (software development or anything else), you will need more. Depending on your work, provide the required free space.

### Step 1: Create a new machine

Launch VirtualBox and click on New on the top tool bar. You can also do this by clicking on Machine on the menu bar and selecting New to create a new machine or with keyboard shortcut Ctrl + N as well.

You will be greeted with a new window to specify the name and operating system for your new machine.

### Step 2: Name and operating system

Enter Ubuntu for name and you should automatically get Linux under Type and Ubuntu (64-bit) on Version. Click Next. You can enter any name and location for the machine.

If you don't get 64-bit option under Version, it's because your system does not support virtualization yet. To change this, boot into BIOS (usually with F2). Turn on virtualization and reboot your system. 64-bit will be available.

### Step 3: Memory Size

This memory refers to the RAM and not hard drive space. Usually, 2 GB is enough. However, you may provide more RAM depending on how much you have. If you are confused, provide half of your RAM (4 GB if you have 8 GB of RAM).

Note that this amount of memory is the maximum amount of memory that your guest can use. The guest machine will use memory in incremental manner, that is, using more when required, up to the specified maximum size. When guest is not running, the host can use all of your physical memory.

### Step 4: Hard disk

This steps adds hard disk to your machine, simulating a hard drive storage in a real physical machine. Since, this is your first installation, choose Create a virtual hard disk now and click on Create.

The recommended space is 10 GB. However, provide at least 20 GB of hard drive storage or more depending on what your purpose is.

- Choose VDI (VirtualBox Disk Image). This works well with VirtualBox and supports storage expansion (or reduction) later.
- Next, choose Dynamically allocated. This storage space is used incrementally as well, when required only.

Note that this represents the maximum amount of disk space the guest OS can use. This is only the upper limit of that space and the actual storage space used by the guest OS is the amount of space its current files are using.

### Step 5: Choose the image file for live boot

- Choose your guest machine on the left panel (Ubuntu in this case). Go to Settings and go to Storage tab.
- Click on the icon on the right (beside IDE Secondary Master) and browse and select your downloaded Ubuntu iso. You will see the name of the image file under Controller: IDE on the left.
- Also check Live CD/DVD so that the guest boots from this CD.
- Click on OK to make these changes.

### Step 6: Boot into Ubuntu

With this setup, we are now ready to boot into your guest OS. Since we checked Live CD/DVD, the guest boots from the image file into Live Ubuntu Session. Click on Start on the toolbar to start this guest machine. When prompted, click on Try Ubuntu and you will be taken to Ubuntu desktop.

Notice that when you are taken to Ubuntu Desktop, the display is smaller. This makes it difficult to work with Ubuntu. Go to Settings inside Ubuntu change the display resolution to fit to your screen and the guest will resize accordingly. The resolution can be fixed later after installation.

### Step 7: Install Ubuntu

Note that by default, Ubuntu is installed in bios mode. This is perfectly fine to be used inside a VirtualBox. However, if you want EFi mode, go to Settings > System > Motherboard of your guest OS and check Enable EFI (Special OSes only). Ubuntu inside VirtualBox supports EFI. Also, you will need to install extension pack for this.

The installation process is the same as if you are installing on a physical machine. Although specific images can be found for some OSes for VirtualBox, manually installing is always a good way to learn. See [here]({% post_url 2022-05-24-install-ubuntu %}) for installing Ubuntu.

### Conclusion
After this, you have successfully installed Ubuntu on your VirtualBox. See [here]({% post_url 2022-05-24-install-ubuntu-on-virtualbox-post-installation-guide %}) for post installation guide where you will learn to set up shared folders, USB usage and auto resize guest display among others.
