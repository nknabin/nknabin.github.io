---
layout: post
title: "How to install VirtualBox on Windows and Linux (Ubuntu)"
date: 2022-05-23 15:30:22 +0545
categories: virtualization
---

This is a complete guide on how to install VirtualBox on Windows and Ubuntu 20.04. There is also a post-installation tips that help you get the most out of VirtualBox. VirtualBox is a Type-2 Hypervisor that allows you to run multiple guest machines on your host machine simultaneously. Depending on which host you are running, the process varies. So, follow the steps below according to your host machine:

### Windows

1. Go to [VirtualBox Downloads page](https://www.virtualbox.org/wiki/Downloads).
2. Click on 'Windows Hosts' to download VirtualBox for Windows.
3. Go to your Downloads folder. Double click on VirtualBox-x.x.x-#####.exe and click **Yes** on the administrator's prompt asking if you want to run the application.
4. Go ahead and finish the installation using default options. Tweak it as you feel necessary.

After installation, you can launch VirtualBox from Start menu.

### Ubuntu 20.04 (Focal Fosa)

Follow one of these ways to install VirtualBox on Ubuntu.

- GUI<br>
Search for **VirtualBox** in **Add or Remove Software** (or similar software centers). Then, click on Install. Finally, apply the changes.

- CLI<br>
Use the package management tool on Ubuntu called **apt** (Advanced Packaging Tool). Linux headers are required for compiling virtualbox package.

  ```
  $ sudo apt update
  $ sudo apt install linux-headers-generic linux-headers-$(uname -r) -y
  $ sudo apt install virtualbox -y
  ```

After this, you are ready to go. Launch VirtualBox by searching in programs or from a terminal by typing virtualbox.

### After installation

#### Add your user to vboxusers group<br>
The group vboxusers is created when VirtualBox is installed. Use the command below to add your user to that group.

```
$ sudo usermod -a -G vboxusers $USER
```

Remember to log out and in again for this change to take effect. You can view your current groups with the **groups** command.

### Install Extension pack

Go to [VirtualBox Downloads page](https://www.virtualbox.org/wiki/Downloads). Find VirtualBox #.#.## Oracle VM VirtualBox Extension Pack. Download the extension pack for your current VirtualBox version or download **All Supported Platforms** extension pack. See VirtualBox version from its Help > About menu. Follow these steps to install the extension pack.

1. Go to File > Tool > Extension Pack Manager.
4. Click on Install. Choose the downloaded file. Let the installation complete.

### Optional: Change default location for guest machines

Another important thing that I do is change the default location for the guest machines. By default, it uses your home partition on Linux and C: drive on Windows. You can do this by navigating to File > Preferences > General. Under the Default Machine Folder, browse and choose another location for you guest machines.

### Conclusion
With all of these, you are ready to use VirtualBox to its full potential. Go ahead and install your first guest machine on VirtualBox. See [here]({% post_url 2022-05-24-install-ubuntu-on-virtualbox %}) to install Ubuntu as a guest machine.
