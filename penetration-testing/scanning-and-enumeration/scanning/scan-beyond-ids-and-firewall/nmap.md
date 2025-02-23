# nmap

Firewalls and IDSs are intended to prevent port scanning tools such as Nmap, from receiving a precise measure of significant data of the frameworks that they are scanning. However, these prevention measures can be easily overcome. Nmap has numerous features that were created specifically to bypass these protections. It has the ability to issue a mapping of a system framework, through which you can view a substantial amount of information, form OS renditions to open ports. Firewalls and interruption recognition frameworks are made to keep Nmap and other applications from obtaining that data.

## Overview of Firewalls Evasion Techniques

A firewall operates on a predefined set of rules. Using extensive knowledge and skill, an attacker can bypass the firewall by employing various bypassing techniques. Using these techniques, the attacker tricks the firewall to not filter the malicious traffic that he/she generates.

Following are some firewall bypassing techniques;

* Port Scanning
* Firewalking
* Banner Grabbing
* IP Address Spoofing
* Source Routing
* Tiny Fragments
* Using an IP Address in Place of URL
* Using Anonymous Website Surfing Sites
* Using a Proxy Server
* ICMP Tunneling
* ACK Tunneling
* HTTP Tunneling
* SSH Tunneling
* DNS Tunneling
* Through External Systems
* Through MITM Attack
* Through Content
* Through XSS Attack

***

## Ping Sweep Scan

Discover the live machines in the network.

```
nmap -sP [network range/CIDR]
```

***

## Packet Fragmentation

Packet fragmentation refers to split probe packet into several smaller packet whilesending it to a network..

```
nmap -f [target host]
```

***

## Source Port Manipulation

Source port manipulation refers to manipulating actual port numbers with commonport numbers to evade IDS/firewall: useful when firewall is configured toallowpackets from well-known port like HTTP, DNS, FTP, etc.

```
“nmap -g [port HTTP/DNS/FTP] [target host]
```

***

## Maximum Transmission Unit (MTU)

With MTU, smaller packets are transmitted instead of sending one complete packet at a time. This technique evades the filtering and detection mechanismenabledinthetarget machine.

```
nmap -mtu 8 [target host]
```

***

## IP Address Decoy

IP address decoy technique refers to generating or manually specifying IPaddressesof the decoys to evade IDS/firewall. This tech make it difficult for IDS/firewall todetermine which IP address was actually scanning the network and whichIPaddresses were decoys.

```
nmap -D RND:10 [target host]
```

***

## MAC Address Spoofing

MAC Address spoofing technique involves spoofing a MAC address with the MACaddress of legit user on the network. Allow send request packets to targetedmachine/network pretending to be a legit host.

```
nmap -sT -Pn --spoof-mac 0 [target host]
```

***

## Zombie Scan

Can choose any of the IPs that are obtained in the ping sweep scan.

```
nmap -sI [IP zombie] [IP target]
```

***

