---
layout: post
title: "How to install Ubuntu: Post installation guide"
date: 2022-05-24 14:20:22 +0545
categories: linux
---

If you stumbled here first, see [here]({% post_url 2022-05-24-install-ubuntu %}) to install Ubuntu.

Here is a list of things that we will do in this tutorial:

1. Change root password.
2. Update and upgrade Ubuntu.
3. Install drivers.
4. Install your favorite browser.
5. Install useful applications.
6. Customize your look and feel.

### 1. Change root password

The account you set up during installation is a normal account. It does not have any superuser (administrative) privileges. The administrative account in Linux (and all UNIX variants) is called the root account, superuser, or simply root. The root user has the highest privilege, and access to all files. Therefore, it controls everything inside Linux.

**passwd** is a command-line utility that allows a user to change his/her password. To change the root password, enter the following command in a terminal: (when you type any password in a terminal, the characters are hidden for security purposes)

```
$ sudo passwd
```

You can use passwd without any arguments to change your own account's password.

Here are a few important things to keep in mind regarding the root account:

- Don't ever log in to the desktop using the root account.
- Use sudo command for all tasks that require administrative privilege.
- Use a strong root password and don't share it with anyone.

### 2. Update and upgrade Ubuntu

First, we are going to add 32-bit (i386) architecture support so that we can install and use 32-bit packages as well.

```
$ sudo dpkg --add-architecture i386
```

Now, update your system.

```
$ sudo apt update && sudo apt upgrade -y
```

The update command only fetches the list for any updates based on repositories defined in /etc/apt/sources.list file. The second command applies any changes found in that list.

You can also update your system using GUI. Open Ubuntu Software by searching for it after pressing the Super key (Windows key is called Super key). Go to Updates. If there are any updates, select which you want to apply and click on Apply to make the changes.

### 3. Install drivers

This step is crucial for those who have Nvidia (or other) graphic cards and other proprietary hardware. The easiest way to add drivers is to open up Software and Updates program.

You can choose the repository, choose how you get updates, and more. We are interested in drivers. Go to Additional Drivers. Let the application search for updates. It will search for updates that your machine requires and list them for you.

Select which drivers you want to install and click on Apply Changes.

### 4. Install your favorite browser

Ubuntu 20.04 comes with Firefox as the default browser. However, you can install any browser that you prefer.

We are going to install Chromium, the open-source browser that's much like Google Chrome. Chromium is provided by the package called chromium-browser. Firefox is provided by the package firefox, if you want that instead of Chromium. There are two ways to install packages. The first way is using a terminal as shown below:

```
$ sudo apt install chromium-browser -y
```

The second way is to use GUI. Open up Ubuntu Software. Search for Chromium browser. Then, click on Chromium. Click on Install and enter your password (not root's). Let the installation finish.

Once installed, Chromium can be launched from Show Applications and searching for it.

### 5. Install useful applications

Even though Ubuntu comes with lots of applications by default, you will want to install more applications that you require. You can install the following packages using apt, snap or Ubuntu Software:

#### Using apt:

VLC - A multimedia player that supports most multimedia files and streaming protocols

```
$ sudo apt install vlc -y
```

#### Using snap:

Viber - Instant messaging app

```
$ snap search viber
$ sudo snap install viber-unofficial
```

#### Using Ubuntu Software:

Visual Studio Code - A favorable general-purpose highly efficient code editor.

Search for Visual Studio Code in Ubuntu Software. Click on Visual Studio Code to go to its description page and click on Install to install Visual Studio Code.

You can search for and install other programs in similar ways.

### 6. Customize your look and feel (Optional)

Gnome Tweak Tool gives you the extra flexibility of customizing your device. It allows you to change appearance, icon, and cursor themes, customize title bars, top bars, fonts, keyboard/mouse behaviors, and more. Install tweak tools using the command below:

```
$ sudo apt install gnome-tweaks -y
```

Also, install gnome-shell-extensions to enable using user shell themes. Shell themes change the appearance of the top and left bars on Ubuntu.

```
$ sudo apt install gnome-shell-extensions -y
```

You can also install this from the browser (Chromium or Firefox) by searching for the same name.

Log out and log back in before moving forward to enable user shell themes.

Install your favorite theme with the following command: (Use apt search to search for your theme.)

```
$ sudo apt install arc-theme -y
```

Open up gnome-tweaks from a terminal or from Applications, turn on User themes in the Extensions tab.

Go to Appearance and select Arc theme under Applications and Shell.

Here are some other things that I like to do:
- Change wallpaper
- Change default screensaver or screen locking and sleep behavior.
- Change default applications for different file types.

Make other changes that you want and reboot!

### Conclusion

You have successfully learned how to install programs, change default settings, change wallpapers, change passwords, and keep your system up to date. Here are some tips to get you started in Ubuntu (or Linux):

- Learn command line. You can use Ubuntu without using command line for most of your daily works. If you want to get into software development or just learn Linux, learning command line is essential.
- Don't rush! Learning Linux and its principles take a lot of time, patience and dedication. If you keep using Linux to get your works done, you will learn along the way.
- Read man pages and help output. Every command you will use is documented. Find those in man (manual) pages. Just run man command or command -h or command --help in a terminal.
