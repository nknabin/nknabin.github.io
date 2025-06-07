---
layout: post
title: "Linux: DPKG and .deb packages"
date: 2022-08-10 10:20:32 +0545
categories: linux
---

dpkg is the package management tool that is used in Debian and its derivatives like Ubuntu and Kali. It is a tool to install, build, remove, extract and analyze debian (.deb) packages. The front-end for dpkg is apt (Advanced packaging tool), which means that apt uses dpkg for package installation, updates, removal and more.

dpkg does not have dependency resolution. When you call apt (or apt-get) install on certain package, apt (or apt-get) analyses the dependency tree and fetches the required files. These files are then passed to dpkg to extract, analyse, and install in correct locations and configure them according to the scripts inside them.

## Before we start

Always use `apt` (or `apt-get`) to install packages from your configured sources. See this tutorial. Use dpkg for packages that are not available via these sources but are distributed by developers as .deb package.

## Install a package

You can install .deb packages by the following command:

```
$ sudo dpkg -i <pkg>.deb
```

For instance, go to https://www.google.com/chrome/ and click on Download. Select 64 bit .deb version. Save it in your Downloads folder (or anywhere else).

Navigate to your Downloads folder and enter the following command (use TAB completion):

```
$ sudo dpkg -i google-chrome-stable_current_amd64.deb
```

Launch google-chrome like any other program. Search for it on Menu (All Applications or press Super key and search).

## List packages

Use one of the commands below to list all packages:

```
$ sudo dpkg -l
$ sudo dpkg --list
```

Use arrow keys to navigate the output. Use Space to jump down a page. Use Ctrl + F and Ctrl + B to move forward and back a page respectively. Use Ctrl + D and Ctrl + U to move down and up half a page respectively.

To search for a keyword, type / (see the left corner at the bottom of the screen). Type a keyword and press enter. It searches for that keyword and brings matching line to the beginning. Use n and N for forward search and backward search for the same keyword respectively.

Use this with `grep`, to see if a certain package is installed.

```
$ dpkg -l | grep google
```

## Remove a package

It removes a package from the system but keeps its modified configuration files.

```
$ sudo dpkg --remove <pkg>
$ sudo dpkg -r <pkg>
```

To remove Google Chrome (google-chrome-stable):

```
$ sudo dpkg -r google-chrome-stable
```

##  Purge a package

It removes a package from the system as well as its configuration files. Use this if you want to do a complete re-installation of a package.

```
$ sudo dpkg --purge <pkg>
```

## dpkg-deb - extract, analyze (and more) a .deb package

`dpkg-deb` provides various options to work with the contents of a deb package. These commands can also be used with dpkg.

### Show information of a .deb package

```
$ dpkg-deb -I google-chrome-stable_current_amd64.deb
$ dpkg -I google-chrome-stable_current_amd64.deb
```

### Show content of a .deb package

```
$ dpkg -c google-chrome-stable_current_amd64.deb
```

### Extract content of a .deb package

```
$ dpkg -x google-chrome-stable_current_amd64.deb
```

The extracted files are the ones that will be installed if you run dpkg -i on the package. Use the extracted files to analyze the package. You can see the structure of the package as well as its configuration files.

You can download .deb files using apt. Enter apt download <pkg> to download .deb file for this package. For example: apt download cmatrix downloads .deb file for cmatrix.

After extraction the package can simply be run without installing. Just find the binary and run it. Since, dpkg can not handle dependency by itself, it does not always work. You will need to manually install dependencies.

This will work for deb packages distributed by developers like the google chrome package we downloaded above. Create a directory called google-chrome and extract the content of the .deb file into this directory.

```
$ mkdir google-chrome
$ dpkg -x google-chrome-stable_current_amd64.deb google-chrome
```

If you see the content of google-chrome directory, you will see that it contains etc, opt and usr directory like in your root hierarchy. These files are the same files that will be copied under root during installation.

To run google-chrome without installing, navigate to google-chrome/usr/bin and execute the following command:

```
$ cd google-chrome/usr/bin
$ ./google-chrome-stable
```

'.' points to current directory. The './' in the second command tells the terminal to run this program from current directory. You can install deb packages on a directory and add path to that directory and use it without installation. However, know that this does not always work and comes with lots of dependency hassle.

## Status of a package

The status of each package is listen in `/var/lib/dpkg/status`. Use `grep` on this file to get status of a specific package. A package may have one of these status: installed, half-installed, not-installed, config-files, half-configured, unpacked and more. (See the man page for dpkg).

Re-configuring a package is required in some cases. One of the most usual ones are interrupted installation/upgrades. Packages can be configured using: (see man dpkg-reconfigure)

```
$ sudo dpkg-reconfigure <package>
```

Use this to reconfigure all packages:

```
$ sudo dpkg --configure -a
```

## Conclusion

dpkg is a useful tool if you are working with deb packages. Use this tool to get information about installed packages as well. However, for installing packages, stick to apt unless you have got a deb package.
