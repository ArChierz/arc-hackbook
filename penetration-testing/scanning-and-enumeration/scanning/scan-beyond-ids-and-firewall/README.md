# Scan beyond IDS and Firewall

Perform network scan without being detected by the network security perimeters suchas the firewall and IDS. This perimeter still have security limitations. Techniques may be required to exploit these limitations: o Packet fragmentation o Source routing o IP Address spoofing.

Techniques to evade IDS/firewall:&#x20;

* **Packet Fragmentation**: send fragmented probe packets to the intended target, which reassembles it after receiving all the fragments.&#x20;
* **Source Routing**: specifies the routing path for the malformed packet toreachthe intented target.&#x20;
* **Source Port Manipulation**: manipulate the actual source port with the common source port to evade IDS/firewall.&#x20;
* **IP Address Decoy**: generate or manually specify IP addresses of the decoysso that the IDS/firewall cannot determine the actual IP address.&#x20;
* **IP Address Spoofing**: change source IP addresses so that the attack appears to be coming in as someone else.&#x20;
* **Creating Custom Packets**: send custom packets to scan the intended target beyond the firewalls&#x20;
* **Randomizing Host Order**: Scan the number of hosts in the target network in a random order to scan the intended target that is lying beyond the firewall&#x20;
* **Sending Bad Checksums**: Send the packets with bad or bogus TCP/UDP checksums to the intended target&#x20;
* **Proxy Servers**: use a chain of proxy servers to hide the actual source of a scan and evade certain IDS. firewall restrictions&#x20;
* **Anonymizers**: Use anonymizers that allow them to bypass the internet censors and evade certain IDS and firewall rules.



