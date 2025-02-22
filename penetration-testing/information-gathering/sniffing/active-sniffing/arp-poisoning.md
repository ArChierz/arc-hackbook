# ARP Poisoning

**ARP Spoofing** is a method of attacking an Ethernet LAN. ARP Spoofing succeeds by changing the IP address of the attacker’s computer to the IP address of the target computer. A forged ARP request and reply packet find a place in the target ARP cache in this process. As the ARP reply has been forged, the destination computer (target) sends the frames to the attacker’s computer, where the attacker can modify them before sending them to the source machine (User A) in an MITM attack.

***

## ARPSpoof

**arpspoof** redirects packets from a target host (or all hosts) on the LAN intended for another host on the LAN by forging ARP replies. This is an extremely effective way of sniffing traffic on a switch.

### PoC

#### Open Wireshark to capture the packets

#### Run the arpspoof

```
arpspoof -i [interface] -t [target IP] [gateway IP]
```

in here, type this:

```
arpspoof -i eth0 -t 10.10.1.11 10.10.1.11
```

The above cmd informs the AP that the target system (10.10.1.11) has our MAC address (MAC of Parrot machine). After sending a few packets, stop sending the ARP packets by press `CTRL+Z` .



now, type this:

```
arpspoof -i eth0 -t 10.10.1.11 10.10.1.1
```

in here, the host system informs the **target system** (10.10.1.11) that it is the **access point** (10.10.1.1).



Observe the ARP packets with an alert warning `Duplicate use of 10.10.1.11 Detected!` . Click on them and observe the MAC address of IP address **10.10.1.1** and **10.10.1.11**&#x20;



Using **arpspoof**, we assigned the MAC address of the host system to the target system (Windows 11) and access point. Therefore, the alert warning of a duplicate use of 10.10.1.11 is displayed.

* You can navigate to the Windows 11 machine and see the IP addresses and their corresponding MAC addresses. You will observe that the MAC addresses of IP addresses 10.10.1.1 and 10.10.1.13 are the same, indicating the occurrence of an ARP poisoning attack, where 10.10.1.13 is the Parrot Security and 10.10.1.1 is the access point.

Attackers use the **arpspoof** tool to obtain the ARP cache; then the MAC address is replaced with that of an attacker’s system. Therefore, any traffic flowing from the victim to the gateway will be redirected to the attacker’s system.

***

## Habu

**Habu**, an open source penetration testing toolkit that can perform various tasks such as ARP poisoning, ARP sniffing, DHCP starvation, and DHCP discovers.

```
habu.arp.poison [target IP] [gateway IP]
```

