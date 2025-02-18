# nmap

## Perform TCP Connect/Full Open Scan and Verbose

This scan will completes a 3-ways handshake

```
nmap -sT -v [target host]
```

***

## Perform Stealth Scan/TCP Half-open Scan and Verbose

This can involves resetting TCP connection before 3-ways handshakeiscomplete. Can be used to bypass firewall, logging mechanism, and hide undernetwork traffic

```
nmap -sS -v [target host]
```

***

## Perform Xmas Scan

Xmas scan sends TCP frame with FIN, URG, and PUSHflag. If the target port is open, then will receive no response. If the target has closedport, thenwill send an RST.

The display showed that the ports either open or filtered on target machine by configured firewall.

```
nmap -sX -v [target host]
```

***

## Perform TCP Maimon Scan

FIN/ACK probe is sent to target. If no response then the target isOpen/Filtered, but if the RST sent as a response, then the port is closed.

```
nmap -sM -v [target host]
```

***

## ACK Flag Probe Scan (firewall-enabled)

This scan sends an ACK probe packet with random seq. number. No responsemean the port is filtered and RST response mean port is not filtered.

```
nmap -sA -v [target host]
```

***

## UDP Scan

No response mean the port is open, and if ICMP port unreachable receivedthen the port is closed.

```
nmap -sU -v [target host]
```

***

## Null Scan

Often long enough to get resp.

```
nmap -sN -v -A -T4 [target host]
```

***

## IDLE/IPID Header Scan

TCP port scan used to send a spoofed source aaddress to a computer todiscover what services are available.

```
nmap -sl -v [target host]
```

***

## SCTP INIT Scan

INIT Chunk sent to target, INIT+ACK response mean port is open, andABORT chunk response mean port is closed.

```
nmap -sY -v [target host]
```

***

## SCTP Cookie Echo Scan

No response implies port is open and ABORT Chunk response mean port is closed.

```
nmap -sZ -v [target host]
```

***

## Detect Service Versions

Help to obtain info. About running service and version. This infocoulddetermine which exploit the target system is vulnerable to.

```
nmap -sV [target host]
```

***

## Scan whole subnet/IP range with Aggressive Scan

Aggressive scan option support OS Detection (-O), version scanning(-sV), script scanning (-sC), and traceroute (--traceroute). **You shold not use this against target network without permission**.

```
nmap -A [target host]*
```

***















