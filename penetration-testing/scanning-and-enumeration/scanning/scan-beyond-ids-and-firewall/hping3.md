# Hping3

## UDP Scan DDoS

```
hping3 [target host] --udp --rand-source --data 500
```

> \--udp: specifies sending the UDP packets to the target host&#x20;
>
> \--rand-source: enables the random source mode&#x20;
>
> \--data: specifies

***

## TCP SYN Request

```
hping3 -S [target host] -p 80 -c 5
```

> –S: specifies TCP SYN request on target machine&#x20;
>
> –p: specifies assigning the port to send the traffic&#x20;
>
> –c: count packet sent to target machine

***

## TCP SYN Flooding DoS

```
hping3 [target host] --flood
```

***

