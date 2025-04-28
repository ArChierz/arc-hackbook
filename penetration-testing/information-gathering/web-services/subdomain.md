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

### Gather Subdomain from crt.sh

reference: [https://gist.github.com/1N3/dec432d14fec84e09733f39669ebca0f](https://gist.github.com/1N3/dec432d14fec84e09733f39669ebca0f)&#x20;

refinement:

```
echo "Enter the domain name"
read TARGET
curl -fsSL "https://crt.sh/?q=%25.$TARGET" | pup "td text{}" | grep "$TARGET" | sort -u > "$TARGET-crt.txt"
```
