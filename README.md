# Hack Wi-Fi
This repo contains every step to hack any Wi-Fi in your surroundings.  

**The basic requirement is having a Linux distro booted in your Machine or in a Virtual Machine.**
   - Using VM requires an external Wi-Fi network adapter that should support monitor mode. (Recommended: Alfa network adapter)
   - Using your machine doesn't require any external Wi-Fi adapter if the internal one supports monitor mode.  

## Install aircrack-ng:  
Arch:
```
sudo pacman -S aircrack-ng
```
Kali:
```
sudo apt-get update
sudo apt-get upgrade aircrack-ng
```
Ubuntu/Debian:
```
sudo apt-get update
sudo apt-get install aircrack-ng
```
Fedora:
```
sudo dnf install aircrack-ng
```

## Check your wireless LAN adapter name:
```
ip addr
```
**Find your LAN adapter name below:**
Here it is wlan0.
```
1: lo: <LOOPBACK,UP,LOWER_UP> mtu 65536 qdisc noqueue state UNKNOWN group default qlen 1000
    link/loopback 00:00:00:00:00:00 brd 00:00:00:00:00:00
    inet 127.0.0.1/8 scope host lo
       valid_lft forever preferred_lft forever
    inet6 ::1/128 scope host noprefixroute 
       valid_lft forever preferred_lft forever
2: enp2s0: <NO-CARRIER,BROADCAST,MULTICAST,UP> mtu 1500 qdisc fq_codel state DOWN group default qlen 1000
    link/ether 04:42:1a:d0:ee:f4 brd ff:ff:ff:ff:ff:ff
3: wlan0: <BROADCAST,MULTICAST,UP,LOWER_UP> mtu 1500 qdisc noqueue state UP group default qlen 1000
    link/ether 90:e8:68:2c:e8:77 brd ff:ff:ff:ff:ff:ff
    inet 192.168.139.57/16 brd 192.168.255.255 scope global dynamic noprefixroute wlan0
       valid_lft 84054sec preferred_lft 84054sec
    inet6 fe80::a3f5:fa53:55c:b0e7/64 scope link noprefixroute 
       valid_lft forever preferred_lft forever
```
