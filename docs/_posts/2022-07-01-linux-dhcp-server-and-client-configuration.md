---
layout: post
title: "How to set up DHCP server in Linux"
date: 2022-07-01 20:10:19 +0545
categories: linux
---

With a dhcp server set up in a LAN, new devices will get IP addresses automatically when they connect to the network. This saves the network administrator the hassle to manually enumerate each device in the network (eg. a new laptop or mobile phone, which is very common these days).

DHCP servers can easily be set up on a local LAN network (or bigger networks as well). A network interface can be set up as a DHCP server. Other devices, i.e., clients will use this server to get a dynamic IP address according to DHCP protocol.

## Before we start

This tutorial assumes you have done at least the following:

- Server - Ubuntu 20.04 Server inside VirtualBox (or similar)
  - static IP address
  - if inside VirtualBox, bridged networking is configured
- Client - Host machine, Another Guest in VirtualBox running in bridged networking mode, or other devices (other laptops or mobile phones).

See the troubleshooting section below if you run into any problems.

## Server Side Configuration

### Step 1: Configure a static IP address

Your server needs to have a static IP address (as good practice and for saving you from lots of trouble of changing IP addresses later, especially in a real environment).

If you haven't, see [here]({% post_url 2022-06-21-use-static-ip-address-in-linux %}) for how to configure an interface to use a static IP address.

### Step 2: Install the dhcp server

On Ubuntu, dhcp server is available via the package called `isc-dhcp-server`. Install this package using `apt`.

```
$ sudo apt update
$ sudo apt install isc-dhcp-server -y
```

### Step 3: Configure the server to listen on the interface configured in step 1

The server needs to use an interface in order to listen to dhcp requests. The interface must be specified if there are more than one interfaces. However, if only one exists, it will be used by default.

Use the same interface, which you used for the configuration of static IP address in step 1.

Edit the `/etc/default/isc-dhcp-server` file and add your interface to `INTERFACES` or `INTERFACESv4` (or `INTERFACESv6` if necessary).

```
$ sudo nano /etc/default/isc-dhcp-server
```

See the output of `ip addr show` or `ifconfig` to see your interfaces if you are not sure.

```
...
INTERFACEv4=”enp0s3”
...
```

### Step 4: Configure the DHCP server

This step provides different configuration options for the server itself. It provides the nameserver that this server and its clients will use, the default lease time, the range of the available IP addresses and so on.

Edit the `/etc/dhcp/dhcpd.conf` file as follows:

```
$ sudo nano /etc/dhcp/dhcpd.conf
```

Find these entries and change them as specified below:

The entry below provides a default domain-name and a domain name server (DNS). The one in the example below is the IP address of the Google DNS. Use the DNS that is available to your ISP. Multiple name servers can be defined as well, separating each nameserver with a comma.

Nameservers that your system uses will be written on `/etc/resolv.conf` file. Use `cat` command on this file to see the name server that you are using. Find these options and edit them or add them at the end of the file.

```
# DNS
option domain-name “example.org”;
option domain-name-servers 8.8.8.8;

default-lease-time 600;
max-lease-time 7200;

# makes server the official dhcp server in the network
authoritative;

subnet 192.168.1.0 netmast 255.255.255.0 {
    range 192.168.1.10 192.168.1.240;
    option routers 192.168.1.254;
    option subnet-mask 255.255.255.0;
    option broadcast-address 192.168.1.255;
}
```

- Subnet
  - range - available IP addresses that this server can lease to clients
  - routers - gateway
  See the output of `ip route show` after `default via` or see the first gateway in the output of `netstat -rn`.
  - subnet-mask - The subnet mask
  - broadcast-address - The broadcast address

#### What range should I use?

Define a sufficiently large range if you are at your home network. The available IP addresses in a typical home network are 192.168.1.1 to 192.168.1.254 in the above example. So, define a large range like 192.168.1.10 to 192.168.1.240. Multiple ranges can be defined as required.

It's good practice to always leave some IP addresses for future use. This can be useful for static configuration if you decide to add servers. Always use IP addresses outside of DHCP range for static configuration.

Other examples include 192.168.1.1 to 192.168.1.100 that leaves IP for 100 other devices. If an IP has already been used, it won't be allocated to a different client until the lease-time expires or the client releases the IP.

### Step 5: Restart the server and check the status of the server

Use the command below to restart the server:

```
$ sudo systemctl restart isc-dhcp-server
```

Use start or stop in place of restart in order to start or stop the server respectively. Use status to see the server status.

If the server has been configured correctly, the output of the status will say: active (running). Also, the log can be seen by using the tail command on `/var/log/syslog` file.

## Client Side Configuration:

There are various options that you can use as clients. Since the server uses bridged networking, it is accessible by all the devices on the network. Hence, here are some options for the clients:

Note that if you are using your host or mobile devices as clients, then the chances are that they have already been connected to this network before. Hence, they will request for the same IP address that that were allocated before (this is how DHCP works). So, if that IP address does not fall in your range, their request will be fulfilled by your router instead of the dhcp server configured above. See troubleshooting for more.

### 1. Host Machine

The host machine can be used as client. However, do not reboot your host machine in order to get the IP because the VirtualBox will also be powered down, hence, there is no dhcp server running.

Instead, use the following commands to ask your system (interface) to renew the IP address. Wait for a while and the dhcp server will allocate them with IP addresses.

#### 1.1 Windows host

```
$ ipconfig /renew
```

#### 1.2 Linux host

```
$ sudo dhclient -r <interface>
```

The interface is usually eth0, wlan0, enp0s3, wlp0s3 and so on. See the output of ip addr show or ifconfig to see your interfaces. Use the one that already has an IP address (this is the one your system is using).

#### 1.3 Mac OS host

```
$ sudo ipconfig set <interface> DHCP
```

The interface is usually en0, en1, en2 and so on. See the output of ifconfig to see your interfaces. Use the one that already has an IP address (this is the one your system is using).

### 2. Mobile devices (smartphones or tablets)

Any smartphones or tablets that has WIFI can be used as a client. Turn the wifi off, wait for a while (ten seconds) and then turn it back on. The phone will get its IP address from the dhcp server.

### 3. Another virtual machine with bridged networking

Ubuntu 20.04 Desktop on another virtual machine is used as a client. By default, Ubuntu is set up to use dhcp server. Therefore, just booting up the client is sufficient to get an IP address from the dhcp server configured above.

If that does not get the DHCP IP, then configure the IPv4 section in Network Settings to use DHCP. For that, go to Settings. Go to Network. Choose Wired or Wireless settings based on your set up.

## See current clients of the DHCP server

To see that this IP was allocated by the dhcp server configured above, the lease file can be viewed. The leases are stored under `/var/lib/dhcp/dhcpd.leases` in the server. This file can be viewed using cat command, among others.

```
$ cat /var/lib/dhcp/dhcpd.leases
```

## Conclusion

The dhcp server was successfully configured and an IP was successfully leased to the client. This is just a minimal configuration and other configuration like binding a fixed IP address to a given client is also possible (this is useful for providing IP addresses to primary and auxiliary servers in the network). Similarly, every other client that connects to the specified network will get an IP address from the valid range configured in the server. This saves the hassle of manual configuration of all new devices in the network.

## Troubleshooting

Open up another terminal and type in `journalctl -f`. It will show the system log in real time so you can see what's happening. Then, reboot the client or restart its interface.  You will see communication between the dhcp server and clients as well. It's helpful to determine what's wrong.

Note that the client requests for a specific IP address if it was allocated that IP address before. These logs can help you identify what is going on.

- If the server won't start, it's usually due to one of these:
  - Incorrect line in configuration file<br>
  Check your configuration file carefully for mistakes like incorrect spelling and missed semicolon.
  - Not configured to listen to any interface<br>
  See if you have used the right interface name in all of your configuration files. Also check if that interface has an IP address.

- There is no lease in leases file, (server is running but client does not use the server)<br>
  - It's usually because all the clients have been connected to this network before and are therefore requesting for the IP address they used before.
  - These IP addresses fall outside the range of your dhcp server and therefore, are fulfilled by the router itself.

  Adjust the range on your server to include that IP address, then restart your client or simply ask for new IP address using the following command:

  ```
  $ sudo dhclient -r <interface>
  ```
  This commands forces the client to renew its IP address. Use the journalctl -f command in terminal to see what happens in the server side.
