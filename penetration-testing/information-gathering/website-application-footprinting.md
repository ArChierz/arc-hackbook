# Website Application Footprinting

## Gather info using ping cmd utility

### Get IP

```
ping domain
```

### Check maximum packet size

```
ping domain -f -l 1300
```

### Check TTL expire

```
ping domain -i 3
```

### Check from n to gain the hop value

```
ping domain -i (n) -n 1
```

***

## Photon

{% embed url="https://github.com/s0md3v/Photon" %}

### Crawl target website for internal and external scripts URLs

```
python3 photon.py -u domain
```

### Crawl target using URLs archive.org

```
python3 photon.py -u http://www.certifiedhacker.com -l 3 -t 200 -wayback
```

Could be use to cloning target website, extract secret keys and cookies, obtain strings.

Could do attack with brute force, DoS, injection, phishing, and SocEng.

***

## Gather Information about Target Website

### Central Ops

### Website Informer

### Burp Suite

### Zaproxy

***

## Extract Company Data

### Web Data Extractor

### Parsehub

### spiderfoot

***

## Mirror Target Website

### HTTrack Website Copier

### Social-Engineer Toolkit (SET)

***

## Gather More Information

### GRecon

to perform finding subdomains, login pages, directory listing, exposed document, wordpress entries.

***

## Recon using WhatWeb

WhatWeb identifies websites ad recognizes web technologies, including content management systems (CMS), blogging platforms, statistics and analytics packages, JavaScript libraries, web servers, and embedded devices. It also identifies version numbers, email addresses, account IDs, web framework modules, SQL errors, and more.

```
whatweb [target]
```

{% embed url="https://github.com/jakbin/whatweb" %}

### Verbose Scan

```
whatweb -v [target]
```



### Generate a report

```
whatweb --log-verbose=[name of report] [target]
```

***

## Web Spidering using OWASP ZAP and Burp Suite Scanner

OWASP Zed Attack Proxy (ZAP) is an integrated penetration testing tool for finding vulnerabilities in web applications. It offers automated scanners as well as a set tools that allow you to find security vulnerabilities manually. ZAP provides functionality for a range of skill levels.

***

## Detect Load Balancers

Organizations use load balancers to distribute web server load over multiple servers and increase the productivity and reliability of web applications. Generally, there are two types of load balancers, namely DNS load balancers (Layer 4 load balancers) and HTTP load balancers (Layer 4 load balancers).

### dig

The result displaying that a single host resolves to multiple IP addresses, which possibly indicates that the host is using a load balancer. dig command provides detailed results and is used to identify whether the target domain is resolving to multiple IP addresses. To determine it, look at the multiple IP in `ANSWER SECTION`

```
dig [target]
```

### lbd

Displayed the available DNS load balancers used by the target website. lbd (Load Balancing Detector) detects if a given domain uses DNS and HTTP load balancing via the Server: and Date: headers and the differences between server answers. It analyzes the data received from application responses to detect load balancers.

```
lbd [target]
```

***

## Directory Brute Forcing

### gobuster

Directory brute forcing to identified web server directories.

In real time, attackers use Gobuster to scan the target website for web server directories and perform fast-paced enumeration of the hidden files and directories of the target web application.

Gobuster is a command-oriented tool used to brute force URIs in websites, DNS subdomains, and names of the virtual hosts on the target server.

```
gobuster dir -u [target] -w [wordlist loc]
```

### Dirsearch

{% embed url="https://github.com/maurosoria/dirsearch" %}

#### Listing all directories of the target website

```
python3 dirsearch.py -u [target]
```

#### Listing all directories with specified extension

```
python3 dirsearch.py -u [target] -e [ext: aspx,php,etc.]
```

#### Listing all dircetories with excluding status code specified

```
python3 dirsearch.py -u [target] -x [code: 200,301,302,404,403,401,500]
```

***

## Identify XSS Vulnerability using PwnXSS

PwnXSS in an open-source XSS scanner that is used to detect cross-site scripting (XSS) vulnerabilities in websites. It is a multiprocessing and customizable tool written in Python language.

{% embed url="https://github.com/pwn0sec/PwnXSS" %}

Try to analyze and confirm the vulnerability that may exist with CRITICAL sign to the web browser.

```
python3 pwnxss.py -u [target]
```

***

## Enumerate WordPress with WPScan

Metasploit Framework is a penetration testing toolkit, exploit development platform, and research tool that includes hundreds of working remote exploits for a variety of platforms. It helps pen testers to verify vulnerabilities and manage security assessments.

Perform multiple attacks on a vulnerable PHP website (WordPress) in an attempt to gain sensitive information such as usernames and passwords. How to use WPScan tool to enumerate usernames on a WordPress website, and how to crack passwords by performing a dictionary attack using an msf auxiliary module.

```
wpscan --api-token [token] --url [full url] --enumerate u
```

***



