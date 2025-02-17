# Website Footprinting

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

