# Active Sniffing

Perform active sniffing on the target network using various active sniffing techniques such as MAC flooding, DHCP starvation, ARP poisoning, or MITM. In Active sniffing, the switched Ethernet does not transmit information to all systems connected through the LAN as it does in a hub-based network.

In active sniffing, ARP traffic is actively injecting into a LAN to sniff around a switched network and capture its traffic. A packet sniffer can obtain all the information visible on the network and records it fir future review. A pentester can see all the information in the packet, including data that should remain hidden.

Active sniffing involves sending out multiple network probes to identify access points. The following is the list of different active sniffing techniques:

* **MAC Flooding**: involves flooding the CAM table with fake MAC address and IP pairs until it is full
* **DNS Poisoning**: Involves tricking a DNS server into believing that it has received authentic information when, in reality, it has not
* **ARP Poisoning**: Involves constructing a large number of forged ARP request and reply packets to overload a switch
* **DHCP Attacks**: Involves performing a DHCP starvation attack and a rogue DHCP server attack
* **Switch port stealing**: Involves flooding the switch with forged gratuitous ARP packets with the target MAC address as the source
* **Spoofing Attack**: involves performing MAC spoofing, VLAN hopping, and STP attacks to steal sensitive information

