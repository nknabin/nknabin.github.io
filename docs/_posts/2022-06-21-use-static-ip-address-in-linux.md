---
layout: post
title: "How to set up Linux (Ubuntu) to use static IP address"
date: 2022-06-21 10:40:09 +0545
categories: linux
---

In the real environment, a server needs to be accessible to many clients within and outside of the organization and even from different parts of the world. Imagine how troublesome it would be if the server changed its IP address every time on boot. It would practically be impossible to use the server. Therefore, a server must have a static (fixed) IP address that does not change with every boot.

This tutorial takes you through the steps required to set up Ubuntu Server to use a static IP address. The steps to do the same in any Linux distribution are similar, depending on what tool you will be using. This tutorial covers the following two methods:

- [`netplan`](#using-netplan-ubuntu-1804-and-higher) (default on Ubuntu)
- [`/etc/network/interfaces`](#using-etcnetworkinterfaces-older-than-ubuntu-1804).

### Using Netplan (Ubuntu 18.04 and higher)

Netplan was introduced in Ubuntu 18.04 and it replaced `ifupdown`. It uses a configuration file inside `/etc/netplan/` with an extension .yaml. These configuration inside `/etc/netplan` persists on reboots.

By default, it has some configuration (that uses dhcp) like this:

```
network:
  ethernets:
    enp0s3:
            dhcp4: true
  version: 2
```

`enp0s3` is the interface. If you are not sure, see the output of `ip addr show` or `ifconfig` command to see what interfaces are available to you (except `lo`, which is a virtual loopback interface that points to itself).

If you see any other options, just keep them as they are.

To configure an interface with a static IP address, use the following configuration: (Edit only the part inside the interface (enp0s3) or similar).

```
$ sudo nano /etc/netplan/00-installer-config.yaml
```

Use the filename that you have under `/etc/netplan/`.

Edit the file to add the following content:

- addresses: The IP address that will be used as static IP address.<br>
It is a good practice to use the lower end IP addresses under 192.168.1.10. Since static IPs are used for servers, it keeps them away from DHCP range, which is usually higher IPs (from good practice again).

- gateway: The gateway in your network.<br>
If you are not sure, see the output of ip route show in your host. The ip after default via is your gateway.
Or if that is not available, you can use the command: netstat -rn. The first gateway is your default gateway.

- namesevers: The addresses of DNS.<br>
The above example uses Google DNS. Some DNS may not be available to you based on your ISP. If you are not sure, see the output of cat /etc/resolv.conf in your host.

```
network:
  ethernets:
    enp0s3:
            addresses: [192.168.1.5/24]
            gateway4: 192.168.1.254
            nameservers:
                    addresses: [8.8.8.8,8.8.4.4]
  version: 2
```

To apply this configuration without rebooting, use the following command:

```
$ sudo netplan apply
```

If you run into some errors, use the following to get more information:

```
$ sudo netplan --debug apply
```

#### Check new ip

Use one of the following to see if the IP address has been updated according to your configuration. The interfaces are named: eth0, wlan0, enp0s1, wlp0s1 and so on. The IP address is similar to 192.168.1.5 after the term 'inet'.

```
$ ip addr show
$ ifconfig
```

### Using `/etc/network/interfaces` (Older than Ubuntu 18.04)

This is an old approach since the newer versions of Ubuntu uses netplan by default (since Ubuntu 18.04). Therefore, the following packages need to be installed on the newer versions.

- `ifupdown` - provides ifup and ifdown commands that uses interfaces definitions on `/etc/network/interfaces` file to configure the interfaces.
- `net-tools` (optional but useful) - provides you some important commands (e.g., `ifconfig` and `net-stat`) relating to the control and management of network in Linux.

```
$ sudo apt update
$ sudo apt install ifupdown net-tools -y
```

Installing `ifupdown` will also generate `/etc/network/interfaces` file. A reboot is necessary. If this file has not been generated, do not continue. Troubleshoot for ifupdown (eg. purge `ifupdown` and install it again).

#### Configure `/etc/network/interfaces`

The generated `/etc/network/interfaces` file typically contains at least the following:

```
# interfaces(5) file used by ifup(8) and ifdown(8)

Append to the /etc/network/interfaces file:

$ sudo nano /etc/network/interfaces

auto enp0s3
iface enp0s3 inet static
address 192.168.1.5
network 192.168.1.0
netmask 255.255.255.0
broadcast 192.168.1.255
gateway 192.168.1.254
```

- `enp0s3` is the name of the interface. See the output of ip addr show or ifconfig command to see what interfaces are available to you (except lo).

- `auto` - configure on boot

- `iface` - interface name

- `inet` - interface uses TCP/IP networking

- `address` - IP address to be used (this will be the IP address of the machine)

- `network` - Network id (ends with #.#.#.0)

- `netmask` - sub net mask

- `broadcast` - broadcast id

- `gateway` - gateway id (The gateway in your network. If you are not sure, see the output of ip route show in your host. The ip after default via is your gateway.)

Don't set network and broadcast in the configuration if you are not sure of them. They are easy to be mistaken. The system will calculate them for you without mistake, if they are not specified here.

For the above change to take effect, do the following:

```
$ sudo systemctl restart networking
```

Alternatively, use ifdown and ifup command to bring down an interface and then bring it back up respectively. When the interface is brought back up, the configuration is read from the above file and applied.

```
$ sudo ifdown <iface> && sudo ifup <iface>
```

Here is an example that uses enp0s3 interface.

```
$ sudo ifdown enp0s3 && sudo ifup enp0s3
```

If you don't get the updated IP, flush the interface first then restart the interface.

```
$ sudo ip addr flush enp0s3
$ sudo ifdown enp0s3 && sudo ifup enp0s3
```

Check your new IP address using ip addr show or ifconfig in a terminal.

### Ubuntu Desktop: Set up static IP address using GUI

Ubuntu Desktop can be configured to use a static IP address from GUI using network settings. Since the desktop versions are mostly used as clients, hence, no static configuration is required. However, if you use this as server, you can set the static IP address using the Settings for your network.

1. From the Network Settings, choose your interface settings (Wired or Wiresless).
2. Under Ipv4, click on Manual.
3. Enter the details (address, netmask, gateway). Others can left as Automatic.
4. Click on Apply. Restart your network using the On/Off switch.
5. This configuration persists on boot as well.
6. Check the new ip address in a terminal or under Details of the same settings window:

### Conclusion

You have successfully configured an interface to get a static IP address every time. Using the configuration files make the configuration persist on reboots.

Don't use both `netplan` and `ifupdown`. Use `netplan` by default. If you want to experiment with `ifupdown`, remove `netplan` (or its configuration file). When the netplan configuration file is not found, the system will look at `/etc/network/interfaces`.

### Troubleshooting

- If you don't get the new IP address as per your configuration file, flush the interface and then restart the interface (bring it down and then up).

- Check for any kind of mistake in configuration files: like incorrect interface name, incorrect IP addressing, incorrect gateway, network or braodcast id.

- If you get the IP but internet doesn't work (e.g., you can not browse or ping google.com), there is a problem with your nameserver. Check in your configuration file if you are using a valid nameserver. Alternatively, test by adding a known valid nameserver (like 8.8.8.8) in your `/etc/resolv.conf` file.

- For netplan, you can use `sudo netplan --debug apply` to see more information on what's wrong.

- If you have both `netplan` and `ifupdown` installed, remember that the system first checks for netplan configuration on boot. Only when the configuration is incorrect or not found, it checks for ifupdown configuration.
