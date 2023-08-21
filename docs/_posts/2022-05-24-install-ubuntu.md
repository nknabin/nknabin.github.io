---
layout: post
title: "How to install Ubuntu"
date: 2022-05-24 12:30:02 +0545
categories: linux
---

### Before you begin

If you are doing this for the first time, you are better off installing Ubuntu 20.04 inside VirtualBox.

1. Backup
Back up your data into another drive or cloud. Since this is going to be a full standalone installation, we will be completely wiping the disk. In any situation, it is recommended to back up your important data before you proceed.

2. USB drive of at least 4 GB
We will use this drive to live boot into Ubuntu and install it.

This is going to be a Ubuntu standalone installation. If you are looking for dual booting Ubuntu with Windows, then installing Windows first is recommended as it simplifies a lot of things. Create enough free space during Windows installation for Ubuntu.

### Download the image file (iso).

Download Ubuntu Desktop from Ubuntu's website. You can either download a LTS (Long Term Support) distribution, which is more stable, or you can download the latest Ubuntu version.

### Make a live bootable USB drive.

The nice part of Linux iso files is that they can be run live from a USB drive, hence called bootable drives. Linux distributions allow themselves to be run via a USB drive without being actually installed on the device. This is called live booting of the distribution. You can use it to test the OS before actually installing it on your hard drive or for troubleshooting (like resetting lost passwords or repairing data).

#### Make a bootable USB drive on Windows

There are various tools to make a bootable USB drive on Windows. We are going to use Rufus. Download a portable version of Rufus from its homepage. Open Rufus and follow the steps below:

1. Choose your USB drive.
2. Choose the image(.iso) file downloaded in step 1.
3. Label the disk if you want to.
4. Click on Start and wait for the process to complete.

Rufus will need to download a **SysLinux** file when used to create a bootable drive for Linux. Confirm when prompted for this.

Also, if you are prompted, select one of the two options from Write in Iso image mode or Write in dd image mode. If you are not sure, go with the recommended one: Write in Iso image mode.

#### Make a bootable USB drive on Linux

Use the dd command-line utility to create the bootable USB drive.

When you run this command, make sure you type the arguments correctly. Otherwise, you risk the chance of breaking your system and also damaging your hard drive. The arguments are explained below:

- if (input file): the path to the downloaded image (iso) file
- of (output file): a usb drive.

Physical drives are often mounted as sdx under /dev (for example: /dev/sda, /dev/sdb, /dev/sdc and so on), with x incrementally replaced from a through z (and more). To find out which is your USB drive, use the fdisk utility.

```
$ sudo fdisk -l
```

Physical drives can be recognized with labels if the drives are labeled. They can also be recognized with the size of the drive. Usually, if you have one hard drive and one USB drive mounted, the hard drive is recognized by the kernel as /dev/sda and the USB drive as /dev/sdb.

Assuming your drive is /dev/sdb, run the following command to make it bootable with the downloaded Ubuntu iso. bs sets the block size for the new drive (USB drive in this case). However, this is not necessary. sync is important because, without it, dd might return before the write operation completes, which can create an incomplete or corrupt USB drive. Make sure that you eject your drive safely.

```
$ dd if=/path/to/ubuntu-20.04.iso of=/dev/sdb bs=4M && sync
```

Wait for the process to complete after which you should see output that shows **the number of records in and out and the number of bytes copied**.

#### Boot Into Ubuntu (BIOS or UEFI)

Use UEFI if you are using any modern hardware, which defaults to UEFI. Use bios if you have old hardware and are satisfied with only one or two primary partitions. Use UEFI if you have modern hardware and will be working with a larger number of partitions (greater than 4 primary partitions) and multiple hard drives.

Follow the steps below to boot into Ubuntu:

1. Plugin the USB device and restart your machine.
2. Press the function key for the boot menu (usually F12 or F8; see for your devices).
3. Select your USB drive label if you wrote any. Otherwise, choose USB drive or a similar option (generally written as the manufacturer's name for the drive or Generic USB drive).

### Start the Installer

Choose to Install Ubuntu when prompted for installation or double click on Install Ubuntu on Desktop. You will be greeted by the installation welcome screen. Choose English or any other preferred language and click on Continue.

### Keyboard layout and Time Zone

WHen prompted for, select the keyboard layout and timezone that works for you. This settings can be changed later as well.

### Updates and other software

Choose normal installation if you want everything that comes with the default Ubuntu Desktop.

Choose minimal installation if you only want minimal programs (no office package, games, and media players).

Disable Download updates while installing Ubuntu if your internet is slow or unstable. You can optionally choose to install third-party software but that's not necessary for now since you can install any programs you want after the installation is complete. Click on Continue after you are done with this.

### Disk Partition

Choose one of the following:

- Erase disk and install Ubuntu<br>
This is recommended for beginners. The installer creates all required file systems.

- Something else (manual partition)<br>
There can be a number of advantages of having different partitions for boot, swap, root, and home. The most important one is that you can keep your personal files separate so that reinstalling Ubuntu if required (or other operating systems) is easier. To do this, click on Something else and click on Continue.

If you are using UEFI, create a new **gpt** style partition table in the installer by clicking on New Partition Table. Use **msdos** style partition table if you are using BIOS.

Create the following partitions:
1. MBR/EFI partition<br>
Usage: Stores boot files and information<br>
Size: 500 MB<br>
File system type: ext2 for BIOS and EFI System Partition for UEFI (If not available, select FAT32 as file system type and set boot flag)<br>
Partition type: Primary<br>
Mount point: /boot

2. Root partition (/)<br>
Usage: Stores system files, binaries, libraries and logs. Everything in Linux starts at /.<br>
Size: at least 20 GB is recommended. 30 GB is enough for most use cases.<br>
File system type: ext4<br>
Partition type: Primary<br>
Mount point: /

3. Optional Swap partition<br>
Usage: Used as additional memory if your RAM is full.<br>
File system type: linux-swap<br>
Partition type: Primary<br>
Mount point:<br>

    If you have an SSD, creating a swapfile after installation will work better as you will have more control of it. If you have 16 GB or more RAM, you do not need a Swap partition unless you are planning to use memory heavy applications. If your RAM is 8 GB or less, create a swap partition of type linux-swap and size 8 GB. For RAM less than 4 GB, create a swap with size 2 x RAM.

4. Home partition<br>
Usage: Stores all user's files including Downloads, Documents, Photos and Videos.<br>
Size: Allocate all remaining space for your home partition.<br>
File system type: ext4<br>
Partition type: Primary<br>
Mount point: /

If you have a large hard drive, you can create additional partitions for other usage (such as backups). If you leave the mount point empty, you can mount them later on on your desired mount point. However, if you are using BIOS (which uses an MBR partition table), you can create only four primary partitions. A GPT partition table (supported by UEFI) allows you to make much more (128 by default).

### Install boot loader
Select /dev/sda (default), i.e. your primary hard drive for boot loader installation. A boot loader is a program that loads the kernel and the root file system and gets your system booted up to the login screen by default.

### Add user info

Add your username and password and continue.

### After installation
After completion, remove the USB drive and reboot your computer. You will be greeted with the Login screen. Login with your credentials.

### Conclusion
Take a look at [post-installation guide]({% post_url 2022-05-24-install-ubuntu-post-installation-guide %}) to tweak Ubuntu to suit your needs.
