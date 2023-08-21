---
layout: post
title: "Common usage of apt"
date: 2022-08-14 12:15:00 +0545
categories: linux
---
apt (advanced packaging tool) is a command-line utility used for managing packages in Ubuntu (any Debian-based distros). The most common uses of apt are to install packages and update your system. This tutorial provides this and some other uses of apt that help beginners get familiar with apt.

## Before we start

There are two ways of installing applications in Linux. The first and the traditional way is to compile the application directly from its source. This is not efficient as it takes time and end-users are not always familiar with the technology. The other way is to install a package. A package can be installed via a GUI program like Ubuntu Software, which uses apt and dpkg packaging system to install programs or by using apt in a terminal. 

A package is a compressed file archive containing all the files that make up an application. Packages are pre-compiled (hence, includes binary files, libraries and configuration files) so that the installation is easy. Think of a package as an executable installer in Windows. It contains information about files for the application and optionally installation instructions and dependency information.

Since a package usually does not contain its dependencies, Linux systems use a package management tool. Ubuntu uses the dpkg package management tool, with apt at its front-end. Such tools make the installation easier since they handle dependencies (downloads and installs required dependencies for a package) on behalf of users, maintains a standard binary format and common installation path.

A package management tool uses a repository to fetch the packages. A repository is like a distribution warehouse for all of the applications for your Linux OS. For Ubuntu, it is defined in the file /etc/apt/sources.list. An element defined in this file is called a source (i.e. a repository) and each source hosts application data (files) and (precompiled) packaged data (files).

apt vs apt-get: Before apt, apt-get was used with dpkg. However, apt-get (and apt-cache) contained many options (low level operations) that are never used by a typical Linux user. Therefore, apt was developed to provide a way of handling packages that is  "pleasant for end-users". It means that it provides the most used options from apt-get (like update, upgrade, install, remove, purge) and apt-cache (search and show). While apt-get can still be used, one should get into the habit of using apt since it is more friendly (added progress bar among others), structured, and provides the most necessary options.

## Update (apt-get update)

apt maintains an index of packages that are installed on your system. apt update re-synchronizes this index (re-downloads the package information) from all configured sources. The other commands uses this data to perform their operations (like upgrading and removing a package).

```
$ sudo apt update
```

Note that this command does not actually upgrade (update) any package but fetches the updated list from the repositories defined in /etc/apt/sources.list. It then uses the updated list to determine if a package should be upgraded, removed or kept the same (no new versions).

You will get the following information depending on the updated index in configured sources (repository):

- HIT: these packages were found on server but haven't been changed
- IGN: there were errors retrieving the files but these are ignored. These errors are regarding the files that don't cause a fatal error, hence can be safely ignored.
- GET: these packages are found on server and have been changed (have updates) (Note that if you run apt update twice, the GET messages will change to HIT because the index has already been downloaded.)

It also shows the number of available updates, if any, as seen above. This feature is not available in apt-get.

You can view the upgradable packages by running the command: sudo apt list --upgradable.

## Upgrade and full-upgrade (apt-get upgrade and apt-get dist-upgrade)

This installs newer version of packages as defined in the updated list fetched by apt update. To upgrade all packages with available updates, type in a terminal:

```
$ sudo apt upgrade
```

This asks for confirmation. Enter y to confirm. To make apt not ask for confirmation, use the -y flag after upgrade (or install).

There is another option called full-upgrade that installs newer versions of packages but also removes packages whenever necessary in order to fully upgrade the system. Use this occasionally to fully upgrade your system. Also, this is required before upgrading Ubuntu to higher versions. 

```
$ sudo apt full-upgrade -y
```

To upgrade your system in a single line, you can use both apt update and apt upgrade as follows:

```
$ sudo apt update && sudo apt upgrade -y
```

You can also use full-upgrade instead of upgrade.

To upgrade only a single package (this is rarely required):

```
$ sudo apt upgrade <pkg>
```

## Install (apt-get install)

To install a package using apt, type in a terminal:

```
$ sudo apt install <pkg>
```

To install multiple packages at once, separate the package names with space.

```
$ sudo apt install <pkg1> <pkg2> <pkg3>
```

If you find ... in the syntax of commands in man pages, eg. for apt install: apt install pkg... , it means that you can provide multiple arguments.

You can install required version with `apt install <pkg>=##.##`. However, you will rarely use this. Stick with the package maintainer's version unless you know what you are doing.

## Remove and purge (apt-get remove and apt-get purge)

To remove a package using apt, type in a terminal:

```
$ sudo apt remove <pkg>
```

You can also remove multiple packages by separating them with space.

This removes all the packaged data (files) but keeps configuration files that have been modified by the user.

To remove the package completely (package files and modified configuration files as well), use purge. You can use purge even on already removed packages to delete their leftover configuration files. This may be essential to bring a package to its original state (especially in the case of server packages like apache and mysql).

```
$ sudo apt purge <pkg>
```

## Search for a package (apt-cache search)

This uses regex terms as search criteria and matches them with package names and their description, so you only need a term that may describe a package. This is useful when you want to install/remove a package but don't know its specific package name.

```
$ apt search <pkg>
```

Note that since it uses regex, it provides a long list of packages that match the name as well as some text in description. To find the actual package you require, read the description. Generally, the package starts with the alphabet itself (eg. apache2 for apache, mysql-server for mysql server), so it's a good idea to navigate to the corresponding alphabet and read descriptions).

For example: Say you want to install apache server but don't know what the actual package is called. So search for it using apt search as follows. Scroll to 'a' and you will find a package with the fitting description as follows. You can use this with grep to make things simpler but that's for later!

```
$ apt search apache
```

After scrolling to 'a', see the package apache2 matching our description: Apache HTTP Server. You can use this package name for installation or other purposes.

## Show information of a package (apt-cache show)

This command shows information about the given package(s) including its dependencies, installation, download size, homepage (if available), sources the package is available from, and description of the package content.

```
$ apt show <pkg>
```

## Autoremove (apt-get autoremove)

This is used to remove packages that were installed as dependencies but are no longer required (due to the change in dependencies or removal of packages needing them). It frees up space that were used by these packages.

```
$ sudo apt autoremove
```

Along with this, you can use apt clean to remove all stored archives in your cache and apt autoclean to remove archives in your cache that can not be downloaded anymore (because they are not available in the repo anymore or have a newer version). However, these are rarely required.

## List (available only on apt)

This command is useful in a number of cases. One that you have already seen is to list upgradeable packages using --upgradable option. Here are some basic uses:

To list all available packages in your repository:

```
$ apt list
```

To list installed packages:

```
$ apt list --installed
```

Combine this with grep to see if a package of interest is installed. For instance, if you want to see if apache2 is installed, type:

```
$ apt list --installed | grep apache2
```

Matching packages will be shown in the output. Empty output means there is no such package named apache2.

To list upgradable packages:

```
$ apt list --upgradeable
```

To list a specific package,

```
$ apt list <pkg>
```

To show all available versions:

```
$ apt list <pkg> --all-versions
```

--all-versions can be used with a specific package as shown above or with apt list --all-versions.

An example for listing all versions of python3 is given below. There is one for amd64 and one for i386 as shown below.

## Conclusion

These highlights the main uses of apt command. After you are familiar, you can dive into commands that provide you more control (eg. apt-file that lets you search for packages that contain a specific file - say a configuration file). However, this is enough for your day-to-day usage.

There is also apt edit-sources that lets you edit `/etc/apt/sources.list`. However, it only opens up the file in an editor so it's not of use for now. Don't edit sources unless you know what you are doing.

See here for troubleshooting common errors while using apt.
