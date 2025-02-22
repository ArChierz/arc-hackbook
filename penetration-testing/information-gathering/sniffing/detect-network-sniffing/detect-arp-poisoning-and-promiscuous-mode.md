# Detect ARP Poisoning and Promiscuous Mode

ARP poisoning involves forging many ARP request and reply packets to overload a switch. ARP cache poisoning is the method of attacking a LAN network by updating the target computer’s ARP cache with both forged ARP request and reply packets designed to change the layer 2 Ethernet MAC address to one that the attacker can monitor. Attacker use ARP poisoning to sniff on the target network. Attackers can thus steal sensitive information, prevent network and web access, and perform DoS and MITM attacks.

Promiscuous mode allows a network device to intercept and read each network packet that arrives in its entirety. The sniffer toggles the NIC of a system to promiscuous mode, so that it listens to all data transmitted on its segment. A sniffer can constantly monitor all network traffic to a computer through the NIC by decoding the information encapsulated in the data packet. Promiscuous mode in the network can be detected using various tools.

To detect this, involves several tools: **Cain & Abel** (PoC), **Wireshark**, **Hping3** (PoC).

***

## Wireshark

### Detect ARP Poisoning in a Switch-based Network

1. Open Wireshark, click **Edit** menu and click Preference&#x73;**...**
2. Expand **Protocols** node, select **ARP/RARP** option, click **Detect ARP request storms** and ensure **Detect Duplicate IP address configuration** is checked.
3. Do the capturing, after a few moment, stop it and click **Analyze** menu and select **Expert Information**
4. Expand the **Warning** node labeled **Duplicate IP address configured (x)**, running on the **ARP/RARP** protocol.
5. Analyze the packets based on the packet number

> * **ARP spoofing** succeeds by changing the IP address of the attacker’s computer to the IP address of the target computer. A forged ARP request and reply packet find a place in the target ARP cache in this process. As the ARP reply has been forged, the destination computer (target) sends frames to the attacker’s computer, where the attacker can modify the frames before sending them to the source machine (User A) in an MITM attack. At this point, the attacker can launch a DoS attack by associating a non-existent MAC address with the IP address of the gateway or may passively sniff the target, and then forward it to the target destination.

***

## nmap

### Detect ARP Poisoning in a Promiscuous mode

```
nmap --script=sniffer-detect [target ip/range]
```

***

## Capsa Network Analyzer

Capsa, a portable network performance analysis and diagnostic tool, provides packet capture and analysis capabilities with an easy to use interface that allows users to protect and monitor networks in a critical business environment. It helps EH or pentester in quickly detecting ARP poisoning and ARP flooding attack and in locating attack source.

{% embed url="https://www.colasoft.com/capsa-free/" %}

