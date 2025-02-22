# MAC Spoofing

A **MAC duplicating** or **spoofing attack** involves sniffing a network for the MAC addresses of legitimate clients connected to the network. In this attack, the attacker first retrieves the MAC addresses of clients who are actively associated with the switch port. Then, the attacker spoofs their own MAC address with the MAC address of the legitimate client. Once the spoofing is successful, the attacker receives all traffic destined for the client. Thus, an attacker can gain access to the network and take over the identity of a network user.

If an administrator doesn’t have adequate packet sniffing skills, it is hard to defend against such intrusions. So, an expert Ethical Hacker and Pentester must know how to **spoof MAC addresses, sniff network packets, and perform ARP poisoning, network spoofing, and DNS poisoning**. Spoof a MAC address to remain unknown to an attacker.

A MAC address is a unique number that can be assigned to every network interface, and it is used by various systems programs and protocols to identify a network interface. It is not possible to change MAC address that is hard-coded on the NIC (Network Interface Controller). However many drivers allow the MAC address to be changed. Some tools can make the Operating System believe that the NIC has the MAC address of user’s choice. Masking of the MAC address is known as MAC Spoofing and involves changing the computer’s identity. MAC spoofing can be performed using numerous tools.

***

## TMAC

{% embed url="https://technitium.com/tmac/" %}

***

## SMAC

{% embed url="https://smac-tool.com/" %}

***

* By spoofing the MAC address, an attacker can simulate attacks such as **ARP poisoning** and **MAC flooding** without revealing their own actual MAC address.

To restore the MAC address back to its original setting, click the Remove MAC button.

***

## MACchanger

### To use this, first turn off the WIFI /LAN connection.

```
ifconfig eth0 down
```

### See available options of **macchanger** tool

```
macchanger --help
```

### see current MAC address

```
macchanger -s eth0
```

> -s: prints the MAC address of the machine

### Change the MAC address of the network interface

```
macchanger -a eth0
```

> a: sets random vendor MAC address to the network interface

### Set a random MAC Address to the network interface

```
macchanger -r eth0
```

> -r: set random MAC

### Enable the WiFi/LAN Connection

```
ifconfig eth0 up
```

