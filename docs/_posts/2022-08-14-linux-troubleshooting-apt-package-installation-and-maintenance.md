---
layout: post
title: "Apt troubleshooting: Package installation, and maintenance"
date: 2022-08-14 16:30:29 +0545
categories: linux
---

apt automatically provides you with some diagnostic information and the solution should intuitively come to you in some cases. See below for more information about common errors while using apt.

## Permission denied

You probably forgot to use sudo. If your user is not in sudoers group, fix that by adding your user to sudoers group.

## Unmet dependencies or broken packages

It is caused due to missing dependencies because the installation/upgrade was stopped before completion.

Enter the command sudo apt -f install or sudo apt --fix-broken install. It will automatically install (correct) the missing dependencies.

## Interrupted dpkg

This erros says: "dpkg was interrupted, you must manually run 'sudo dpkg --configure -a' to correct the problem."

The error occurred because dpkg was interrupted and it did not finish configuring a package(s). Run the command stated above. It re-configures each package, therefore finishing the interrupted process.

## Could not get lock, Unable to acquire lock (or similar):

This means that there is another `apt` (or `apt-get`) process that's currently using apt or dpkg. The other applications that could be using apt are GUI applications like Ubuntu Software or terminals that are open and currently running apt. If you have automatic upgrades, then the system may be trying to run unattended upgrades (run by system automatically without user intervention).

The order you should try to solve this error is the following:

1. Wait for the currently running apt processes to finish
2. Kill stuck processes
3. Reboot
4. Removing lock files or killing dpkg itself

The first approach to this error should be to wait for the processes to finish. If it's running unattended upgrades or if another terminal is running apt, wait for it to finish. Stopping an active installation/upgrade may leave the packages in inconsistent state.

However, sometimes processes are halted and get stuck before they are finished and that leaves dpkg in a locked state. To solve this, kill these processes using the command below.

Use ps with grep to determine what processes are using apt. It gives a lots of columns as its output. The most important ones are the first column (owner of the process), the second column (PID) and the last column (the command it is running).

```
$ ps aux | grep apt
```

If you see the grep command on the right side of the output, this is the above command we just entered with apt in its argument (grep apt) and not a process that's using apt. This is usually the last line in the output of the above command.

Or, to get only the PID and command of processes that are using apt, use pgrep:

```
$ pgrep -a apt
```

Kill the processes using:

```
$ sudo kill <PID>
```

You can also do the following:

```
$ sudo pkill apt
```

If the processes are not killed, use -9 option with kill. This sends SIGKILL signal to the process which causes immediate program termination because SIGKILL can not be handled or blocked by the process. More on this later!

```
$ sudo kill -9 <PID>
```

If this does not work for you, then try rebooting the system. It should remove any stuck processes.

Only if this doesn't work, follow the steps below to kill dpkg.

### Kill dpkg

You can kill an active dpkg process or remove the lock files manually using rm command. However this is not recommended since it may leave packages in inconsistent (corrupted) state.

Use ps with grep to find the PID of dpkg and use kill to kill the process as shown above. Or you can use pkill. Use kill with -9 option to force kill if the process is still persistent.

```
$ ps aux | grep dpkg
$ sudo kill -9 <PID>

# or use this
$ sudo pkill dpkg
```

Manually remove lock files: The file that was locked and is causing this error will be given to you with the error (look at the line that says: can't acquire or can't lock). Remove this file using rm.

Don't remove all of the files but only those that your error says can't lock or acquire or open.

```
$ sudo rm /var/lib/dpkg/front-end
$ sudo rm /var/lib/apt/lists/lock
$ sudo rm /var/cache/apt/archives/lock

$ sudo rm /var/lib/dpkg/lock
```

After this, you must run to avoid any problems:

```
$ sudo dpkg --configure -a
```

This re-configures the packages and if they are in inconsistent state, they will be re-configured to their consistent states.
