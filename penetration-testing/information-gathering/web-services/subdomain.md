# subdomain

{% embed url="https://www.netcraft.com/" %}

### amass



#### Basic Enumeration

```
amass enum -d [domain]
```

passive enum

```
amass enum -passive -d [domain]
```



### sublist3r

```
sublist3r -d [domain]
```

## Pentest-Tools Find Subdomains



### Subfinder

subfinder -d \[domain] -all -recursive > subdomain.txt



### Hakrawler

{% embed url="https://github.com/hakluke/hakrawler" %}

basic

```
echo [domain] | hakrawler
```

#### check specific for subdomain

```
echo [domain] | hakrawler -subs
```



### GAU

{% embed url="https://github.com/lc/gau" %}

