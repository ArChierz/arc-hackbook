# SX tool

Command line scanner can be used to perform ARP scans, ICMP scans, TCPSYNscans, UDP scans, and application scans such as SOCS5 scans, Docker scans, andElasticsearch scans.

## ARP Scan

```
sx arp 10.10.1.0/24
```

***

## ARP Scan JSON Format

```
sx arp 10.10.1.0/24 --json | tee arp.cache
```

***

## List all open TCP port

```
cat arp.cache | sx tcp -p 1-65535 10.10.1.11
```

***

## List all open UDP port

```
cat arp.cache | sx udp -p 1-65535 10.10.1.11
```

According to RFC1122 (Type):

* Destination Unreachable: Code 2
* Port Unreachable: Code 3 According to RFC792 (Code): - Network Unreachable: Code 0
* Host Unreachable: Code 1
* Protocol Unreachable: Code 2
* Port Unreachable: Code 3

{% hint style="info" %}
if the output not displayed, the port is open.
{% endhint %}

