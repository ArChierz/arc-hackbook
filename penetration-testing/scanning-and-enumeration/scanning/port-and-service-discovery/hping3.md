# Hping3

## ACK Scan

ACK Scan send ACK probe to target. **No response** mean **port is filtered**. RST response mean **port is closed**.

```
hping3 -A [IP] -p [port] -c [count]
```

> -A: specifies setting ACK flag
>
> -p: specifies port to be scanned
>
> -c: specifies packet count

***

## SYN Scan

```
hping3 -8 [no.scan mode. could range] -S [IP Host] -V
```

> -8: specifies scan mode

***

## FIN, PUSH, URG Scan

FIN, PUSH, URG scan port target ip, if port is open will receive a response, If port is closed will return RST.

```
hping3 -F -P -U [IP] -p [port] -c [count]
```

> -F: setting FIN flag
>
> -P: setting PUSH flag
>
> -U: setting URG flag
>
> -c: packet count
>
> -p: specifies port to be scanned

***

## TCP Stealth Scan

TCP packet sent to target, if SYN+ACK response received, it indicateportsare open.

```
hping3 --scan [scan no. mode] -S [IP]
```

> \--scan: specifies port range to scan
>
> -S: specifies SYN flag

***

## ICMP Ping Scan

```
hping3 -1 [IP Target] -p [port] -c [count]
```

> -l: specifies ICMP Ping Scan

***

## Scan Entire subnet for live host

```
hping3 -1 [target subnet] --rand-dest -l eth0
```

***

## UDP Scan

```
hping3 -2 [IP target] -p [port] -c [count]
```

















