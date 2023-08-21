---
layout: post
title: "How to install Ubuntu on VirtualBox: Post installation guide"
date: 2022-05-24 18:10:02 +0545
categories: virtualization
---

If you stumbled here first, see [here]({% post_url 2022-05-24-install-ubuntu-on-virtualbox %}) to install Ubuntu inside VirtualBox.

This tutorial can also be used for other guest machines. By the end of this tutorial, you will have done the following things:

1. Enable Auto Resize guest display as the window size changes.
2. Create a shared folder between your host and guest.
3. Enable Shared clipboard between your host and guest.
4. Enable Drag and drop between your host and guest.
5. Work with USB.

### Before you start

Make sure you have installed an extension pack that matches the version of your VirtualBox. If you have not done this, see [here]({% post_url 2022-05-23-install-virtualbox %}) for installing VirtualBox with an extension pack.

This features also require guest additions to be installed inside your guest OS. It gives a guest OS necessary tools to work with above mentioned features. The easiest way to do this is as follows:

1. Click on Devices on the menu bar.
2. Click on Insert Guest Additions CD Image. You should get a pop up asking to run this software like this.
3. Click Run. Enter your password when prompted for. This will open up a new terminal and install the guest additions.
4. Press Return (Enter key) when the installation is completed and reboot (either type reboot on a terminal or choose Power off from the drop down menu and click on Restart).

If the you don't get the popup after on step 2, do the following to find and run guest additions for Linux:

```
$ cd /media/username/VBox_Gas_x.x.x # substitute with real path or use tab completion
$ sudo ./VBoxLinuxAdditions.run
```

Reboot after this and you are ready to go.

### Auto resize guest display

This makes your guest display adapt when you resize the guest window without the need to change the resolution every time. It makes your workflow seamlessly flawless. Check the checkbox on Auto-resize Guest Display in View menu and reboot.

### Shared folders

This is the most useful extended feature of VirtualBox. Since guest machines are isolated in their own environment (like another physical machine), they can not view or edit host files unless specified. This feature allows you to share folders between host and guest.

First we are going to create a folder in our host machine. This folder is going to be shared among our host and guest. We will call this folder sharedoc. You can name it anything you want or share any existing folder by providing the path to that folder.

On host,

```
$ cd
$ mkdir sharedoc
$ cd sharedoc
$ echo "Share me!" > shared.txt
$ cat test.txt
```

The command echo "Share me" > shared.txt creates a file called shared.txt with "Share me!" as its content. We will use this file to verify that we are sharing the right folder.

If you are on Windows, create a New Folder and name it sharedoc and create a new file called shared.txt with any content.

On guest,

To be able to work with shared folders, you will need to add your user to vboxsf group. To do this, enter the following command in a terminal:

```
$ sudo usermod -aG vboxsf $USER
```

 Also, we will create a new folder inside our guest OS that we will use as mount point for the share folder. We will create a folder called vmshare inside /mnt. This means that the contents of the sharedoc folder on host will be available inside /mnt/vmshare.

You can create the folder for mount in other convenient locations like your home folder or even Desktop.

Now, go to Settings of your guest machine and navigate to Shared Folders tab. Note that there are two types of shared folders.

- Machine folders are shared until you manually remove them.
- Transient folders are temporarily mounted and automatically removed on shut down or reboot.

Use the toolbar on the right to add a new shared folder under Machine Folders like shown below:

- Folder Path: the path on host to the folder you want to share (provide absolute path in Linux or browse and choose your file)
- Folder Name: the display name in the guest OS
- Read-only: makes the folder not write-able (handy when you are using guest only as a server and using your host for development)
- Auto mount: mounts the specified folder automatically on guest boot
- Mount Point: The shared folder will be mounted here (the content of the shared folder will be available from this path).
- Make Permanent: The shared folder will be automatically remounted on reboots. Without this checked, the shared folder will no longer mount itself on reboots and you have to manually mount them.

If you have Windows as your guest OS, it will automatically mount your folder as a separate drive. You won't need to specify a mount point.

Now, reboot your system and navigate inside /mnt/vmshare inside your guest and you will see the files. You can add, modify and delete new files and folders as you are working directly on your host. The changes can be seen from host as well.

### Shared clipboard and Drag and Drop

Being able to copy items (not files or folders) and drag and drop items between host and guest is a huge benefit and it makes your workflow very efficient. To enable this follow the following steps:

1. Go to your guest Settings.
2. In General, go to Advanced tab.
3. Under Shared Clipboard and Drag'n'Drop, choose Bidirectional.

### Work with USB

This features makes it possible to work with USB devices that are connected to your physical machine. It can sometimes come in handy when you have to work with infected drives. Follow the following steps to work with USB drive:

1. Plug in your USB drive on your machine (physical machine of course).
2. On your guest, go to Devices on the menu bar.
3. Click on USB. Your drive should be listed there.
4. Click on your drive's name. Your USB drive will have a check mark then (this means your USB drive is mounted) and you will get notified as follows:

To eject drive and use it on host again, eject your drive from guest and uncheck your drive from Devices > USB.

Note that just like a drive can be connected to one physical machine, your drive can be connected to either host or guest but not both at the same time. To use the drive on your host (other guest machines), go to Devices > USB and click on your drive again.

### Conclusion and Tips

You have successfully created shared folders and worked with USB and shared clipboards among others. Keep in mind that you can do all of these using the status bar (the bar at the bottom of the window).

Here are some tips that will make your workflow more efficient:

- Use shared folders efficiently. You can even mount your entire home drive as shared folders (mount /home/username as shared folders).

- Make shared folders read-only if you don't need to modify the content from inside the host. This prevents any unintentional or accidental modifications.

- If you can't share a folder, it's probably because you haven't added your user to vboxsf group or the path to the shared folder is incorrect. Try manually mounting your shared folder in order to test this.

  ```
  $ sudo mount -t vboxsf sharename /mnt/vmshare
  ```

- Instead of shutting down your guest machine, you can save its state and simply get back to that state. The machine automatically loads the saved state on you click Start.

- Working with USB drives can be pretty useful when you are suspicious of the files inside the drive. However, keep in mind that your shared folders may be affected. So, it's best to unmount your shared folders while working with infected drives.
