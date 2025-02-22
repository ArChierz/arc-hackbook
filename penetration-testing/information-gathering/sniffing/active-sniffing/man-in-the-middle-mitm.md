# Man-In-The-Middle (MITM)

An attacker can obtain usernames and passwords using various techniques or by capturing data packets. By merely capturing enough packets, attackers can extract a target’s username and password if the victim authenticates themselves in public networks, especially on unsecured websites. Once a password is hacked, an attacker can use the password to interfere with victim’s accounts such as by logging into the victim’s email account, logging onto PayPal and draining the victim’s bank account, or even change the password.

As a preventive measure, an organization’s administrator should advice employees not to provide sensitive information while in public networks without HTTPS connections. VPN and SSH tunneling must be used to secure the network connection. An expert EH and pentester must have sound knowledge of sniffing, network protocols and their topology, TCP and UDP services, routing tables, remote access (SSH or VPN), authentication mechanisms, and encryption techniques.

Another effective method for obtaining usernames and passwords is by using **Cain & Abel** to perform MITM attacks.

An MITM attack is used to intrude into an existing connection between systems and to intercept the messages being exchanged. Using various techniques, attackers split the TCP connection into two connections—a client-to-attacker connection and an attacker-to-server connection. After the successful interception of the TCP connection, the attacker can read, modify, and insert fraudulent data into the intercepted communication.

***

## Abel & Cain

**Cain & Abel** is a password recovery tool that allows the recovery of passwords by sniffing the network and cracking encrypted passwords. The ARP Poisoning feature of the Cain & Abel tool involves sending free spoofed ARPs to the network’s host victims. This spoofed ARP can make it easier to attack a middleman.

{% embed url="https://github.com/xchwarze/Cain" %}

it can sniff on the target network, can steal sensitive information, prevent network and web access, and perform DoS and MITM attacks.

