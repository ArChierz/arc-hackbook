# nmap

## ARP Ping Scan

Probe ARP request to target host; ARP response mean active host.

```
nmap -sn -PR [Target IP Address]
```

> -sn: disable port scan
>
> -PR: Perform ARP ping scan

***

## UDP Ping Scan

Send UDP packet, a UDP response mean the host is active.

```
nmap -sn -PU [Target IP Address]
```

> -pu: Perform UDP ping scan

***

## ICMP Echo Ping Scan

Sending ICMP ECHO request to a host, if the target is alive, the host returnICMP ECHO reply. Could be useful to determine if ICMP passing throughafirewall.

```
nmap -sn -PE [Target IP Address]
```

> -PE: Perform ICMP Echo Ping Scan

***

## ICMP Echo Ping Sweep

Determine live hosts from range of IP Addresses by sending it to multiple hosts

```
nmap -sn -PE [Target IP Address range]
```

***

## ICMP Address Mask Ping Scan

Alternative for ICMP ECHO ping scan, could be use to determine live host ifadmin block ICMP ECHO pings.

```
nmap -sn -PM [Target IP address]
```

***

## TCP SYN Ping Scan

Send empty TCP SYN to target, ACK response mean the host is active.

```
nmap -sn -PS [Target IP Address]
```

***

## TCP ACK Ping Scan

Technique send empty TCP ACK to target host, RST response meanhost isactive.

```
nmap -sn -PA [Target IP Address]
```

***

## IP Protocol Ping Scan

Tehcnique send different probe packet of different ip protocol to target host, any response mean the host is active.

```
nmap -sn -PO [Target IP Address]
```

***



















