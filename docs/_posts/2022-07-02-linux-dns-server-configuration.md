---
layout: post
title: "How to set up DNS server and use it via client in Linux"
date: 2022-07-02 16:02:00 +0545
categories: linux
---

DNS (Domain Name System) is the Internet service that maps a domain name to its IP address and vice versa. It is the base of Internet browsing because it enables us to remember sites using hostnames and not the actual IP addresses of the servers. When you enter google.com in your browser, your DNS server comes into play to determine the corresponding IP address of google.com. Imagine having to remember the actual ip addresses of all the servers. It would make the Internet not useable at all. Therefore, DNS server is an important piece of the web. This tutorial will take you through the steps to set up a minimal DNS server and test it with A records and CName records.

## Before we start

Make sure you have the following:

- Server - Ubuntu 20.04 Server inside VirtualBox (or similar)
  - static IP address
  - if inside VirtualBox, bridged networking is configured
- Client - Host machine or another Guest in VirtualBox running in bridged networking mode
  - The client is connected using Bridged Networking in VirtualBox.

See the [troubleshooting](#troubleshooting) section if you run into any problems.

## Step 1: Install the server

We will be using BIND (Berkeley Internet Name Domain), an open source domain name server. It is the most popular DNS server used in the world today. It is currently on version 9. On Ubuntu, install the following packages:

- `bind9` - an Internet Domain Name Server
- `bind9-doc` - documentation for BIND
- `dnsutils` (`bind-utils` for RedHat derivatives) - client packages provided with BIND

Install these packages with apt as follows:

```
$ sudo apt update
$ sudo apt install -y bind9 bind9-doc dnsutils
```

## Step 2: Configure DNS server: create zones

The `named.conf` file is the main configuration file for BIND. It tells the server about the zones. Since, it is a global configuration file, you should edit `named.conf.local`, which is included in the named.conf file (see the content of this file).

Add the following content (see below) for both forward and reverse zone at the end of `named.conf.local` file. Anything after // is a comment and you don't have to type those.

### Forward zone

- domain name (practice.local): The domain you are going to use DNS for
- type: master or slave for that domain
- file: the actual file where the information is stored for that domain
- allow-update: For Dynamic DNS as it contains addresses of hosts that are allowed to submit updates for master zones. It is set to none by default.

```
zone "practice.local" IN { // Domain Name

        type master; // Primary DNS

        file "/etc/bind/forward.practice.local.db"; // Forward lookup file

        allow-update { none; }; // Since this is primary DNS, it should be none.

};
```

### Reverse zone

The name of the reverse lookup zone is the first three octets of IP address of the net in reverse order, followed by `.in-addr.arpa`. Replace the IP address with the correct subnet.

For example, if the server has an IP address 192.168.1.5, then the name of the reverse lookup zone is 1.168.192.in-addr.arpa.

```
zone "1.168.192.in-addr.arpa" IN {

        type master; // Primary DNS

        file "/etc/bind/reverse.practice.local.db"; // Forward lookup file

        allow-update { none; }; // Since this is primary DNS, it should be none.

};
```

## Step 3: Create zone lookup file

Each zone defined above needs to have their configuration file at the right location defined above. This file stores the actual information (DNS records) for the zone.

Here are some information that will help you understand the context:

- IN: Internet
- SOA: (Start of Authority) marks the start of a zone of a authority
- NS: an authoritative name server
- MX: Mail Exchange
- A: a host address
- CNAME: the canonical name for an alias
- PTR: a domain name pointer; used for reverse look up since it points an IP address to a host name. Every A record must have a PTR record.

Note that every domain name in this file should end in a dot. A standalone `@` is used to denote the current origin. An `ORIGIN` is the domain name that is appended to any domain names that don't end in a dot. Specifying an origin will replace the current origin. Also, if no `ORIGIN` is specified, `@` refer to the name of the zone.

An origin can be specified as: `$ORIGIN <domain-name> [<comment>]`.

### Forward zone

Copy the file `db.local` to `forward.practice.local.db` inside `/etc/bind.`

```
$ sudo cp /etc/bind/db.local /etc/bind/forward.practice.local.db
```

Any line that starts with ';' are comments. Always end domain names with a dot.

The first 6 lines are the configuration for the zone. This information contains the following:

- zone: What is your domain? (practice.local)
- who is responsible for this zone: root.practice.local (equivalent to root@practice.local)
- Serial number: Keeps track of when this file is updated. You must increment this number whenever you make changes to this file. Otherwise, the server will face errors. A good serial number can be: 2020072701 which shows that this is the first change on 2020-07-27.
- Others: How often to refresh the database, how often to retry a zone transfer, when the zone information will expire and a default time to live. Leave all of these as default.
- Name Server Information: The primary DNS server and its IP address in the next line. Multiple DNS servers can be defined.
- Mail Exchanger:  The mail server responsible for accepting mails on behalf of this domain. The "10" in the MX line states a priority (lower number has higher priority). Multiple mail exchangers can be defined, in which case the mail exchange with lower MX listing is used first.
- Records: Other entries are DNS records (hosts and their IP addresses).

When you update (make any change to) zone files, you must update the serial number on the file. Otherwise, the server will run into errors later.

```
$ sudo nano /etc/bind/forward.practice.local.db
```

Update the file to match to following content:

Note that the IP address used for the authoritative nameserver (NS record) must be the IP address of the server itself (that is, the IP address in static configuration of the server; see the output of ip a or ifconfig).The IP addresses used for other A records can be any IP address inside the subnet. As long as the client can reach the authoritative nameserver, other IP addresses will be resolved.

```
;
; BIND data file for local loopback interface
;
$TTL    604800
@       IN      SOA     practice.local. root.practice.local. (
                              2         ; Serial
                         604800         ; Refresh
                          86400         ; Retry
                        2419200         ; Expire
                         604800 )       ; Negative Cache TTL
;
; Comment the three lines below:
;@      IN      NS      localhost.
;@      IN      A       127.0.0.1
;@      IN      AAAA    ::1

; Name Server Information

@       IN      NS      ns1.practice.local.

; IP address of Name Server

ns1     IN      A       192.168.1.5

; Mail Exchanger

practice.local. IN      MX      10      mail.practice.local.

; A-Record Hostname to IP addres

www     IN      A       192.168.1.6
mail    IN      A       192.168.1.7

; CName Record

ftp     IN      CNAME   www
```

The following records are created:

- NS Record
Authoritative name server - 192.168.1.5
A RecordA host name mapped to  an IP address.
www.practice.local - 192.168.1.6
mail.practice.local - 192.168.1.7

- CName Record
ftp.practice.local as alias for www.practice.local

### Reverse zone

Copy the file `db.127` to `reverse.practice.local.db` inside `/etc/bind`.

```
$ sudo cp /etc/bind/db.127 /etc/bind/reverse.practice.local.db
```

It is similar to the forward zone lookup file except that the entries have the last part of the IP address first and then the hostname last.

You must use the fully qualified domain name and put the dot (.) at the end of each domain name. Otherwise, the server will face errors. Another way to get around this is to use $ORIGIN.

When you update (make any change to) zone files, you must update the serial number on the file. Otherwise, the server will run into errors later.

```
$ sudo nano /etc/bind/reverse.practice.local.db
```

Update the file to match the following content:

```
;
; BIND reverse data file for local loopback interface
;
$TTL    604800
@       IN      SOA     practice.local. root.practice.local. (
                              2         ; Serial
                         604800         ; Refresh
                          86400         ; Retry
                        2419200         ; Expire
                         604800 )       ; Negative Cache TTL
;
; Comment the two lines below:
;@      IN      NS      localhost.
;1.0.0  IN      PTR     localhost.

; Name Server Information

@       IN      NS      ns1.practice.local.

; Reverse Lookup for Name Server

5       IN      PTR     ns1.practice.local.

; PTR Record IP address to Hostname

6       IN      PTR     www.practice.local.
7       IN      PTR     mail.practice.local.
```

## Step 4: Check your configuration

Use `named-checkconf` to check if there are any syntax errors in the BIND configuration file (named.conf*). There will be no output if there are not any errors in the files.

```
$ sudo named-checkconf
```

Use named-checkzone to check syntax errors in zone lookup files. If errors are present, they will be presented to you. Otherwise, the output will say loaded serial followed by the current serial number on your zone file.

### Forward zone:

```
$ sudo named-checkzone practice.local /etc/bind/forward.practice.local.db

zone practice.local/IN: loaded serial 2
OK
```

### Reverse zone

```
$ sudo named-checkzone 1.168.192.in-addr.arpa /etc/bind/reverse.practice.local.db

zone 1.168.192.in-addr.arpa/IN: loaded serial 2
OK
```

Restart bind server with command `sudo systemctl restart bind9`.

## Step 5: Verify DNS server

To verify that the DNS server is working, set up a client machine to use this DNS server. You can use your host machine as the client. You can also use the same server to test the DNS server itself.

Edit the `/etc/resolv.conf` file:

Update the nameserver entry to match your DNS server (the IP address of the NS record, i.e., authoritative name server) :

```
nameserver 192.168.1.5
```

Note that this is good just for testing. The content of `/etc/resolv.conf` is overwritten at boot. The correct way to set up DNS server in clients is to configure the DHCP to use the DNS server (edit the server itself or the dhclient configuration file) or specify the DNS server manually with static IP in netplan or ifupdown configuration file. (You can also use tools like resolveconf.)

Note that if you use specify this name server in `/etc/resolv.conf` file, you will face slow or no Internet issues because this DNS server only contains a few records and you have only specified this DNS server. However, you can verify the DNS server. If you come to this, change your DNS back to what it was or simply reboot.

Use `dig` or `nslookup` command to verify the DNS server. Install `dnsutils` (or `bind-utils` in RedHat derivaties), if you get 'command not found' error.

### Forward lookup
Use `dig <name>`. See the `ANSWER SECTION` for the reply from the server. Here are some examples:

```
$ dig www.practice.local

; <<>> DiG 9.16.3 <<>> www.practice.local
;; global options: +cmd
;; Got answer:
;; WARNING: .local is reserved for Multicast DNS
;; You are currently testing what happens when an mDNS query is leaked to DNS
;; ->>HEADER<<- opcode: QUERY, status: NOERROR, id: 60399
;; flags: qr aa rd ra; QUERY: 1, ANSWER: 1, AUTHORITY: 0, ADDITIONAL: 1

;; OPT PSEUDOSECTION:
; EDNS: version: 0, flags:; udp: 4096
; COOKIE: 149596d40822e513010000005ef6e0d423baa8ce4ef5498b (good)
;; QUESTION SECTION:
;www.practice.local.            IN      A

;; ANSWER SECTION:
www.practice.local.     604800  IN      A       192.168.1.6

;; Query time: 0 msec
;; SERVER: 192.168.1.5#53(192.168.1.5)
;; WHEN: Sat Jun 27 11:46:08 +0545 2020
;; MSG SIZE  rcvd: 91
```

The DNS server resolves www.practice.local to 192.168.1.6.

#### Testing the CNAME record:

```
$ dig ftp.practice.local

; <<>> DiG 9.16.3 <<>> ftp.practice.local
;; global options: +cmd
;; Got answer:
;; WARNING: .local is reserved for Multicast DNS
;; You are currently testing what happens when an mDNS query is leaked to DNS
;; ->>HEADER<<- opcode: QUERY, status: NOERROR, id: 55432
;; flags: qr aa rd ra; QUERY: 1, ANSWER: 2, AUTHORITY: 0, ADDITIONAL: 1

;; OPT PSEUDOSECTION:
; EDNS: version: 0, flags:; udp: 4096
; COOKIE: b40f5de2be97fa52010000005ef6e13941766d0ad7d34303 (good)
;; QUESTION SECTION:
;ftp.practice.local.            IN      A

;; ANSWER SECTION:
ftp.practice.local.     604800  IN      CNAME   www.practice.local.
www.practice.local.     604800  IN      A       192.168.1.6

;; Query time: 0 msec
;; SERVER: 192.168.1.5#53(192.168.1.5)
;; WHEN: Sat Jun 27 11:47:49 +0545 2020
;; MSG SIZE  rcvd: 109
```

The DNS server resolves ftp.practice.local as a CNAME record for www.practice.local and resolves the IP address of www.practice.local as well.

### Reverse lookup
Use `dig -x <ip-address>`.

```
$ dig -x 192.168.1.6

; <<>> DiG 9.16.3 <<>> -x 192.168.1.6
;; global options: +cmd
;; Got answer:
;; ->>HEADER<<- opcode: QUERY, status: NOERROR, id: 11829
;; flags: qr aa rd ra; QUERY: 1, ANSWER: 1, AUTHORITY: 0, ADDITIONAL: 1

;; OPT PSEUDOSECTION:
; EDNS: version: 0, flags:; udp: 4096
; COOKIE: 765a956bc2c511e3010000005ef6e0fc23ef76507b905a23 (good)
;; QUESTION SECTION:
;6.1.168.192.in-addr.arpa.      IN      PTR

;; ANSWER SECTION:
6.1.168.192.in-addr.arpa. 604800 IN     PTR     www.practice.local.

;; Query time: 0 msec
;; SERVER: 192.168.1.5#53(192.168.1.5)
;; WHEN: Sat Jun 27 11:46:48 +0545 2020
;; MSG SIZE  rcvd: 113
```

The IP address 192.168.1.6 is mapped to www.practice.local which is correct as per the configuration file.

## Conclusion

The DNS server has successfully been configured. A-records and CName records are successfully tested. Note that this is a general configuration. The server is much more flexible than this. Refer to its documentation (if you installed bind9-doc, it can be found in /usr/share/doc/bind9/ location) to configure it to suit your needs.

Here are some resources that will help you further customize the server and get full understanding of it:

- bind9 doc: /usr/share/doc/bind9 (from bind9-doc)
- [ISC's BIND Site](https://www.isc.org/bind/)
- [RFC: Domain Names - Implementation and Specification](https://datatracker.ietf.org/doc/html/rfc1035)

## Troubleshooting

If you get any errors in configuration files, check for missing semicolons and missing dot at the end of the domain names.

If you get no answer section, check to see if you are using the correct DNS server in `/etc/resolv.conf`. Also, check if the IP address for the authoritative name server (the one with NS record) is up and running.
