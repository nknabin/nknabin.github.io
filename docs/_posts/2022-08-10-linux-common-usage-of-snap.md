---
layout: post
title: "Common usage of Snap"
date: 2022-08-10 13:11:22 +0545
categories: linux
---

Snaps (snap packages) are containerized software packages that are easy to install, secure, cross-platform and dependency free. They are auto-updated. Since they contain their dependencies, they run on all major Linux platforms without modification. You just need to install snapd on your Linux distribution and you can work with snaps. The commands are independent of distributions (they are the same for all distributions).

Snaps are managed by snapd service that runs in the background. snap is the command line interface to help you install, remove, upgrade (and more) snaps. This tutorial provides some common usage of snap.

snap is the name given to both the application package format (.snap) and the command line utility to manage these packages.

Snaps are better at security, handling dependencies and updating apps. Snaps are auto-updated. Since snaps ships with its own libraries and dependencies, they have no dependency issues. However,  Since snaps contain their dependencies, they are usually larger in size than deb packages. So, they are compressed to solve this problem, which occasionally makes them smaller than deb packages.

## How are snaps more secure?

A snap is isolated in its own area, i.e. it is not installed like a .deb package. They are just snap archives (SquashFS Filesystem archive: compressed read-only file system for Linux) that sit on disk as read-only images. They are mounted as a loopback device and decompressed at runtime with a private write area for each snap. By default, snaps run on strict confinement (isolation) which means that they are given read/write access to only their installation directory and snaps can not access other snaps and the underlying system.

Some snaps are available in classic confinement and they behaves as a traditional packages. These snaps, unlike isolated snaps, need to read and write system (root) files. However, these packages are safe as these are approved by a team at snapcraft.io.

## Before we start

Install `snapd`. On Ubuntu, it is installed by default. Type `snap --version` in a terminal to check its version. By default, some snaps (like core) are installed. These provide minimal system environment on which your snaps will run.

Continue with the tutorial after snapd is installed. Note that this works with distributions other than Ubuntu as well.

## Find a snap

The find command searches the store for available packages. To search for a package, use the following command:

```
$ snap find <snap>
```

This will also work with snap search <snap>

For example: to search for vlc:

```
$ snap find vlc
```

## Install a snap

The install command installs snaps in the system.

```
$ sudo snap install <snap>
```

To install vlc,

```
$ sudo snap install vlc
```

To install classic snaps (like sublime-text, use the --classic flag):

```
$ sudo snap install sublime-text --classic
```

## List installed snaps

This displays a summary of snaps installed in the system.

```
$ snap list
```

## Refresh (Upgrade) a snap

Snaps are updated automatically. An update is called a refresh. You can manually update your snaps as well. The refresh command refreshes (updates) a snap (or all if none specified).

### To get available updates:

```
$ sudo snap refresh --list
```

### To refresh all snaps:

```
$ sudo snap refresh
```

### To refresh only one snap:

```
$ sudo snap refresh <snap>
```

For example: to refresh vlc:

```
$ sudo snap refresh vlc
```

## Revert (downgrade) a snap

The revert command reverts (downgrade) the specified snap to previous state (revision). Do this when you are unhappy with the updated version.

```
$ sudo snap revert <snap>
```

## Remove a snap

This removes a specified snap from the system. By default, it saves a snapshot (data and configuration) of this snap before removing it. You can add --purge option to remove the snap without saving a snapshot. This can be useful if you want to do a clean reinstallation of snaps.

```
$ snap remove <snap>
$ snap remove --purge snap <snap>
```

## Disable/Enable a snap

This disables/enables a snap in the system. When a snap is disabled, its binaries and services are unavailable for use (you can not run this application), but all the data is still available. Enabling a disabled snap takes a single command.

To disable and enable a snap:

```
$ sudo snap disable <snap>
$ sudo snap enable <snap>
```

For example: to disable vlc:

```
$ sudo snap disable vlc
```

To enable vlc again:

```
$ sudo snap enable vlc
```

## Alias/Unalias a snap

The alias command sets up an alias (another name) to a given snap. You can now use this alias name to call the snap instead of its original name. The unalias command removes this alias.

This can be useful when working with commands that are long and a hassle to type every time.

```
$ sudo snap alias <snap> <alias>
$ sudo snap unalias <snap>
```

Alias intellij-idea-community (which is long and difficult to remember) to idea (which is short and easier to remember). Remove the alias and idea no longer works.

## See snap system changes

This shows the summary of changes made to the system recently.

```
$ snap changes
```

## Conclusion

Snaps are getting more popular. Although they have their issues (slow run time among others), they do a much better job at security, dependency resolution and updates. Get into a habit of using snaps as it lets you get the same experience across multiple distributions, which can be really helpful. And if you want to develop your own snaps, learn snapcraft.
