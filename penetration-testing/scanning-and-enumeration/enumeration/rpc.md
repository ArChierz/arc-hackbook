# RPC

## NetScanTools Pro

RPC Enumeration. Search for \*nix RPC info, fill the target hostname with IP address and click **Dump Portmap**. Result displaying the RPC infor of the target machine.

Enumerating RPC endpoints enables attackers to identify any vulnerable services on these service ports. In networks protected by firewalls and other security establishments, this portmapper is often filtered. Therefore, attackers scan wide port ranges to identify RPC services that are open to direct attack.

***

## nmap

```
nmap -T4 -A [target]
```

> -T4: specifies the timing template&#x20;
>
> -A: Aggressive scan, option support OS detection (-O), version scanning(-sV), script scanning (-sC), and traceroute (--traceroute)



