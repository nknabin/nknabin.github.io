---
layout: post
title: "How to set up NFS server in Linux"
date: 2022-07-02 12:15:39 +0545
categories: linux
---

NFS or Network File System is the distributed file system protocol that allows a system to access remote directories over the network, just like accessing a local file system. It allows you to store your files in a different location (server) and read or write to that space from multiple clients. It is also useful in organizations where the shared resources must be accessed regularly. However, it should be noted that the protocol is not itself encrypted. Hence, when using NFS over a public network, a VPN or other encrypted tunnels are required to protect your data.

## Before we start

Make sure you have the following:

- Server - Ubuntu 20.04 Server inside VirtualBox (or similar)
  - static IP address
  - if inside VirtualBox, bridged networking is configured
- Client - Host machine or another Guest in VirtualBox running in bridged networking mode
  - The client is connected using Bridged Networking in VirtualBox.

See the [troubleshooting](#troubleshooting) section if you run into any problems.

## Step 1: Installing packages

### Host (server)

Install the package `nfs-kernel-server` that provides the NFS server for sharing (exporting) the files and directories using NFS protocol.

```
$ sudo apt update
$ sudo apt install nfs-kernel-server
```
 
### Client

Install the `nfs-common` package that provides NFS functionality to access the shared files and directories.

```
$ sudo apt install nfs-common
```

Once the required packages are installed, proceed to configure them.

## Step 2: Creating the share directories on the host

We will share two separate directories to show how NFS mounts (mounted directories) can be used. By default, NFS server prevents any superuser actions on NFS mounts since NFS mounts are not part of the client file system. Therefore, the client can not write any file as superuser, change ownership of the files or perform any other superuser actions.

Although the default superuser restriction provides security, it may be desirable for some clients to perform superuser actions, even if they never require superuser privilege on the host itself. This can be achieved with NFS server but it comes with a security implication. Since this allows the client to gain superuser privilege on the entire host system itself (not only on the NFS mounts), this should only be allowed to a certain specific trusted clients only.

### Example 1: A general directory

We will share a directory called nfsshare that can be accessed by anyone over the network. This will later be mounted with the default superuser restriction, therefore, not allowing clients to perform any superuser actions. Such configuration can be used to share files written by server (for instance: uploads from content management systems) to different clients.

Create a directory named nfsshare:

```
$ sudo mkdir -p /var/nfs/nfsshare
```

This directory is owned by root since we used sudo to create it. Verify this using ls command.

```
$ ls -ld /var/nfs/nfsshare
drwxr-xr-x 2 root root 4096 Jul 12 20:01 /var/nfs/nfsshare/
```

By default, NFS translates any root requests from the client to nobody:nogroup for security reasons. Therefore, change the ownership of this folder to nobody:nogroup as follows:

```
$ sudo chown nobody:nogroup /var/nfs/nfsshare
```

Verify that the ownership has changed with ls command.

```
$ ls -ld /var/nfs/nfsshare
drwxr-xr-x 2 nobody nogroup 4096 Jul 12 20:01 /var/nfs/nfsshare/
```

### Example 2: Home directory on the host

Since the home directory already exists, it is not necessary to create the home directory. Also, don’t change the permissions for the home directory on the host as that will lead to problems in the host.

The home directory will later be mounted as a read-write file system, with also removing the default superuser restriction by the NFS server. Sharing any directory with no superuser restriction comes with a critical security risk as mentioned above, therefore, we will make this accessible to a certain number of clients.

With the directories set up, proceed to configure the host to export these directories.

## Step 3: Configuring the exports on the server

The `/etc/exports` file holds the configuration for the directories that are exported (shared) by the host. It also specifies the clients that can access them and other options for exporting.

Edit the `/etc/exports` file:

```
$ sudo nano /etc/exports
```

Add the following content:

```
/var/nfs/nfsshare *(ro,sync,no_subtree_check)
/home 192.168.1.75(rw,sync,no_subtree_check,no_root_squash)
```

- nfsshare
  - \* - Accessible to everyone
  - ro - Read only file system
  - sync - forces NFS to write changes on host as well before replying (every write to a file is written on the server before returning the system call to the user), therefore, creating a more stable environment with the host and client
  - no_subtree_check - disable subtree checking that helps improve reliability in some circumstances involving host and client (multiple clients) simultaneous operations (especially, renaming files) (Enabling this tends to cause more problems than what it does; almost always, export a directory with this disabled)

- Home
  - client_ip - accessible to only this IP address
  - rw - readable and writable file system
  - no_root_squash - prevents NFS from transforming a root request to a non-root request, which is the default behavior. This helps maintain ownership over the shared files and directories. Since this allows the clients to gain root access over the host as well, this should be used only with trusted clients.

After writing the changes, it is necessary to restart the NFS server.

```
$ sudo systemctl restart nfs-kernel-server
```

Check the status of the server using `systemctl status nfs-kernel-server`, which should be active.

## Step 4: Adjusting the firewall on the host to let nfs traffic through

Adjust your firewall to allow nfs traffic to come through. For example, if you are using an EC2 instance, you will need to add rules in its security group to allow tcp and udp traffic on port 2049. Navigate to security group of your instance and edit inbound rules to add the required rules.

If you use ufw as a firewall, you can add the following rule to enable nfs traffic from any address.

```
$ sudo ufw allow from any to any port nfs
Rule added
Rule added (v6)
```

If you get an error, replace nfs with 2049. ufw recognizes nfs even though it is not in its list because it checks `/etc/services` for the port and protocol. However, you can use 2049 to specify the rule as well.

Note that this opens nfs traffic to any IP address. This is not desirable in a professional environment. Replace `any` with an IP address to allow only that IP address to send nfs traffic or a subnet to allow all IP addresses within that subnet to send nfs traffic to this machine.

Here is an example that allows nfs traffic to come through one specific client (192.168.1.75):

```
$ sudo ufw allow from 192.168.1.75 to any port nfs
Rule added
Rule added (v6)
```

Use the command ufw status to see currently active rules:

```
$ sudo ufw status
Status: active

To                         Action      From
--                         ------      ----
2049                       ALLOW       Anywhere
2049 (v6)                  ALLOW       Anywhere (v6)
```

Now that the firewall allows nfs traffic, mount the exported directories on client machine.

## Step 5: Mount points and mounting directories on client

On client, create the necessary mount points for both nfsshare and home directories respectively.

```
$ sudo mkdir -p /nfs/nfsshare
$ sudo mkdir -p /nfs/home
```

These are the mount points where the shared directories will be mounted. Any content, if they currently exist on the mount point, will be hidden when the directories are mounted. Therefore, create a new mount point or move the existing content somewhere else.

Mount the hosted directories: /var/nfs/nfsshare and /home with the following commands respectively.

```
$ sudo mount 192.168.1.5:/var/nfs/nfsshare /nfs/nfsshare
$ sudo mount 192.168.1.5:/home /nfs/home
```

192.168.1.5 is the IP address of the server. Check that the directories have been mounted with `df -h` command. The h flag makes the sizes of drives in the output human readable.

```
$ df -h

192.168.1.5:/var/nfs/nfshare    20G  5.7G   13G  31% /nfs/nfsshare
192.168.1.5:/home               20G  5.7G   13G  31% /nfs/home
```

The last two mounts show the NFS mounts.

The used and available sizes mentioned by df represent that of the whole partition that the shared directory resides at in the host machine. To view the space actually used by files on the directories, use the du -sh command. The s flag provides only a summary (rather than the space usage of every file in the directory) and the h flag makes the output human readable.

```
$ du -sh /nfs/home

64K     /nfs/home
```

Now that the drives are successfully mounted, proceed to testing the NFS access.

## Step 6: Testing NFS Access

### Example 1: A general directory

The first test is for the nfsshare directory. It is hosted as a read-only system.  Therefore, create a new file called test.txt on the host and add some content on it. Then verify the content from the client machine.

On host, create a file called test.txt:

```
$ sudo nano /var/nfs/nfsshare/test.txt
```

Add any content (e.g. this is your added content in server) to the file as you like and save it.

On client, use the cat command to verify the ownership of the file defaults to nobody:nogroup and that the same content is available to the client. Furthermore, client superusers are not allowed to perform any super user actions on the file (such as changing ownership).

```
$ cat /nfs/nfsshare/test.txt
```

As seen above, the file written from the host is accessible to the client.

### Example 2: Home directory on the host

The second test is for the home directory. Since it is writable and also preserves the ownership of files, create a file as a root user (using sudo). Then, verify the ownership of the files as shown below:

On client, as a superuser (using sudo), create a test.txt file inside /nfs/home:

```
$ sudo touch /nfs/home/roottest.txt
```

On host, verify the ownership of the files using the ls as shown below:

```
$ ls -l /home
total 0
-rw-r--r-- 1 root  root  0 Jul 12 20:48 roottest.txt
```

As seen above, the ownership of the files are preserved (because this directory was exported using no_root_squash option; without this option, the ownership of the files created using superuser would default to nobody:nogroup). It makes managing user accounts much more convenient, without having to give the client root accounts superuser access on host.

## Step 7: Auto mount at boot

The instructions for mount on boot are stored in /etc/fstab file. Update the file to match the following content for nfsshare and home directories respectively.

Edit the `/etc/fstab` file on client and add the following content:

```
192.168.1.5:/var/nfs/general    /nfs/general   nfs auto,nofail,noatime,nolock,intr,tcp,actimeo=1800 0 0
192.168.1.5:/home               /nfs/home      nfs auto,nofail,noatime,nolock,intr,tcp,actimeo=1800 0 0
```

The fields respectively represent the device (drive), mount point, file system type, mount options, backup (dump) operation and file system check order. Remove the auto from the mount options if you don't want it to automatically mount on boot. You can then mount it manually with the specified options.

Read the man page for nfs to read about the mount options used above.

Now, the client will automatically mount the remote partitions (devices or drives) on boot, if the host is up and running.

## Step 8: Unmount the shared drives

Move out of the NFS mounts and use the umount command as follows to unmount the drives.

```
$ cd
$ sudo umount /nfs/nfsshare
$ sudo umount /nfs/home
```

The output of `df` command should not list the shared drives now that they have been unmounted.

## Conclusion

We have successfully exported two directories using NFS server. Remember that this is only a minimal configuration. Read the man pages for nfs for more detail about available options and use the ones that suit your purposes.

# Troubleshooting

In case of any errors, check your work as follows:

- See if the necessary packages are installed on both the server and the client.
- See if the server is running and is reachable from the client machine (use ping to test this).
- Check to see if you changed the ownership of the exported directory as required.
- Look for any mistakes in the configuration files.
- Check if your firewall is letting through the nfs traffic. If you are using AWS EC2 instances for this, open up both TCP and UDP connections on port 2049 on both instances' security groups.
- Check that the mount points are correct.

