# nmap

## Packet Fragmentation

Packet fragmentation refers to split probe packet into several smaller packet whilesending it to a network..

```
nmap -f [target host]
```

***

## Source Port Manipulation

Source port manipulation refers to manipulating actual port numbers with commonport numbers to evade IDS/firewall: useful when firewall is configured toallowpackets from well-known port like HTTP, DNS, FTP, etc.

```
“nmap -g [port HTTP/DNS/FTP] [target host]
```

***

## Maximum Transmission Unit (MTU)

With MTU, smaller packets are transmitted instead of sending one complete packet at a time. This technique evades the filtering and detection mechanismenabledinthetarget machine.

```
nmap -mtu 8 [target host]
```

***

## IP Address Decoy

IP address decoy technique refers to generating or manually specifying IPaddressesof the decoys to evade IDS/firewall. This tech make it difficult for IDS/firewall todetermine which IP address was actually scanning the network and whichIPaddresses were decoys.

```
nmap -D RND:10 [target host]
```

***

## MAC Address Spoofing

MAC Address spoofing technique involves spoofing a MAC address with the MACaddress of legit user on the network. Allow send request packets to targetedmachine/network pretending to be a legit host.

```
nmap -sT -Pn --spoof-mac 0 [target host]
```

***

