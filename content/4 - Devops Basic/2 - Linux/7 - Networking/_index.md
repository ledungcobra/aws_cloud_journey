---
title: "Networking"
date: "`r Sys.Date()`"
weight: 7
chapter: false
pre: " <b> 7. </b> "
---

- [DNS](#dns)
  - [Search for domain](#search-for-domain)
  - [Record Types](#record-types)
  - [Tool to test DNS](#tool-to-test-dns)
- [Switching](#switching)
  - [Setup network computer 1](#setup-network-computer-1)
  - [Setup network computer 2](#setup-network-computer-2)
- [Routing](#routing)
  - [Setup a linux host as Router](#setup-a-linux-host-as-router)
  - [Note](#note)


## DNS
* Allow you to connect to other host by using DNS name instead of IP address.
* Instead of each time we have new server we need to add ip address to `/etc/hosts` file all we need to do is to host a `DNS server` that maintains a list
of DNS record.
* The client need to configure `/etc/resolv.conf`
```
nameserver DNS_SERVER_IP
```
* If you have duplicated DNS name in `/etc/hosts` and DNS server the order is determined by the file `/etc/nsswitch.conf`
![NSSwitch File](./images/image-5.png)

* `files` stand for the  `/etc/hosts`, `dns` stands for DNS server.
* You can add nameserver hosted by google `8.8.8.8` to the file `/etc/resolv.conf` so that you can ping to many well known websites.

### Search for domain
* /etc/resolv.conf
```
nameserver 192.168.1.1
nameserver 1.1.1.1
search mycompany.com
```
* The `search` directive allows you to search for domain in the current directory.

### Record Types
- A: store alias for mapping IP address to domain name.
- AAAA: store alias for mapping IPv6 address to domain name.
- CNAME: store alias for mapping domain name to another domain name. Example: `www.google.com` is alias for `google.com`.





### Tool to test DNS
* `nslookup`
Example result should look like this:
![NSLookup result](images/_index.png)

* `dig`
Example result should look like this:
![Dig result](images/_index-1.png)

## Switching
* This creates a network for systems to connect with each other.

![Switch](./images/image-1.png)

### Setup network computer 1
* Get network interface 
```shell
ip link
```
![Check network interface name](./images/image.png)
* Add address to VIF
```shell
ip addr add 192.168.1.10/24 dev eth0
```

### Setup network computer 2
* Get network interface 
```shell
ip link
```
![Check network interface name](./images/image-2.png)
* Add address to VIF
```shell
ip addr add 192.168.1.11/24 dev eth0
```

* Now we can run ping command to test connectivity from computer 1
```shell
ping 192.168.1.11
```

## Routing
* To allow computers in different networks to communicate with each other we can use `Router`

![Router](./images/image-3.png)

* To checking the list of routes available in the computer run the below command
```shell
route
```
* The system A (left) and the system B(right). If the system B want to talk with system A it need to configure route

```shell
ip route add 192.168.2.0/24 via 192.168.1.1
```
* The command allows the system B to access the System A via the Gateway at `192.168.1.1`

* To allow route traffic to internet use command
```shell
ip route add default via 192.168.2.1
```
- To delete default route run the below command
```shell
sudo ip r del default
```


### Setup a linux host as Router
![Image 1](./images/image-4.png)
* We have the computer B sit between computer A and C, we need to make computer A and computer C can talk to each other.
* From the computer A it can configure to send traffic to CIDR block `192.168.2.0/24` through Gateway `192.168.1.6`
```shell
ip route add 192.168.2.0/24 via 192.168.1.6
```

* The same config can apply in computer C so that it can send traffic to CIDR block `192.168.1.0/24` thorugh Gateway `192.168.2.6`
```shell
ip route add 192.168.1.0/24 via 192.168.2.6
```

* But this setting still is not enough for Computer A `ping` to computer B vice versa. We need explicit allow forward traffic in computer B
* We can allow it by running the below command
```shell
sudo sh -c "echo 1 > /proc/sys/net/ipv4/ip_forward"
```

* But the setting will be lost when we restart the system, to make this work well we need to update in the `/etc/sysctl.conf` file.

```
/etc/sysctl.conf
...
net.ipv4.ip_forward = 1
...
```


### Note 

```shell
ip link # show interfaces
ip addr # show addresses of interfaces
ip addr add 192.168.1.10/24 dev eth0 # Set ip for network inteface
```
Note that the changes are not persist after reboot.
- To make it persist after reboot we need to add the changes to `/etc/network/interfaces` file.

```
/etc/network/interfaces
...
auto eth0
iface eth0 inet static
    address 192.168.1.10
    netmask 255.255.255.0
...
```
- In some case the network interface is down, to check the status of the network interface run the below command
![Network Interface Is Down](images/_index-2.png)
- To bring the network interface up run the below command
```shell
sudo ip link set dev eth0 up
```
