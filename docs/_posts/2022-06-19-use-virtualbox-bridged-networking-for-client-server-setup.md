---
layout: post
title: "How to use bridged networking to set up client and server lab environment on VirtualBox"
date: 2022-06-19 14:35:21 +0545
categories: virtualization
---

There are a number of configurations that will allow you to practice with client/server architecture using VirtualBox. By default, four network adapters can be used by a single virtual machine. Using different adapter settings, we can configure the virtual machine to be accessible from host, other virtual machines and other devices in the network. Even if you have low ram or an old CPU, the configurations below should be usable with some tweaks.

Here are some of the configurations:

- Use your host as server and virtual machine as client.
- Use virtual machine as server and host as client.
- Use virtual machines for both server and client (If you desire a different OS for your server/client).

No matter which configuration you use, it is necessary for your server and client to be able to reach (or talk to) each other in order to use them as server/client. This tutorial provides bridged networking configuration since it can provide the closest simulation of a real physical server/client in your network.

### Before we start

This tutorial assumes that you have at least one virtual machine set up with Ubuntu (Server) installed. Otherwise, see [here]({% post_url 2022-05-24-install-ubuntu-on-virtualbox %}) to install Ubuntu on VirtualBox.

Also, make sure you understand basic networking modes and the power on/off options that VirtualBox provides. See [here]({% post_url 2022-05-26-virtualbox-basic-features %}) for more.

### Bridged Networking

Bridged networking mode connects your virtual machine on your network as any other physical machine (like another PC, laptop, mobile phone or other devices connected via Ethernet cable or WiFi). This means that your virtual machine will be connected on the same network as your host and other devices. Therefore, it gives you the same environment as working with a real physical server in your network. This is as close as it gets to practicing in a real environment.

#### To use bridged networking:

1. Go to Settings of your virtual machine.
2. Go to Network tab.
3. Select one of the adapters.
4. Under **Attached to**, select Bridged Adapter from the drop down menu. Leave all the other settings as they are. VirtualBox will select correct settings for your machine.
Reboot your virtual machine. It will now use bridged networking mode.

After rebooting, use ifconfig to see your IP address. It will usually be 192.168.#.##, which is the same as your host.

### Client options

If you are using another virtualbox machine as a client, follow the same methods above to make it use bridged networking mode as well.

### Accessing server from host and other virtual machines

Now that your server (and client) machines use bridged networking mode, it can be reached (or accessed) from your host and other virtual machines. To do so, you will first need to know the ip address of the server machine (or client machine if you want to access it).

Use ip command to check the IP address.

```
$ ip addr show
```

lo is a special virtual interface that the system uses to communicate with itself (i.e., 127.0.0.1 points to the system itself).

Alternatively, you can also use `ifconfig` command to see the IP address as follows, if `net-tools` are installed.

```
$ ifconfig
```

Now, from your host machine (or other machines on the same network), use ping to see if your server is available.

```
$ ping <server's ip>
```

Here is an example:


If everything is successful, you will the server replying to this ping request. Otherwise, you will see that the server is not reachable.

### Conclusion

You have successfully configured the guest machine to use bridged networking mode. With it, you can simulate your guest as being connected to the network like any other physical device. You can think of it as having another physical server in your home network.

