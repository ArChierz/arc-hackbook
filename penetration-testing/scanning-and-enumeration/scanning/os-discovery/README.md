# OS Discovery

Identifying this allow to assess the system’s vulnerabilities and the exploits might work. OS Discovery/Banner Grabbing is a method used to determine the OS that is runningon a remote target system.

Two types:&#x20;

* Active Banner Grabbing: crafted packet sent to remote OS, the response is noted and compared to db to determine the OS.&#x20;
* Passive Banner Grabbing: Banner grabbing from error messages, sniffing the network traffic, and banner grabbing from page extensions.

<figure><img src="../../../../.gitbook/assets/image (3) (1).png" alt=""><figcaption></figcaption></figure>

***

## Identify target system's OS with TTL and TCP Window Size using Wireshark

Network protocol analyzer allow capturing and interactively browsing traffic runningon a comp. network. Could be use to identify target oS (Sniffing and Capturing). Could observe TTL and TCP window size to determine OS.

* ping the host
* check the IP ICMP packet with open the IPv4 tab
* identify the **Time to Live** value
