# nmap

Could extract considerable valuable information from target system. With NSEcouldobtain information such as OS, Computer name, domain name, forest name, NetBIOScomp. name, NetBIOS domain name, workgroup, system time of a target system, etc.

## Aggressive Scan

Provide information about open ports and running services along versions andtarget details such as OS, computer name, NetBIOS computer name, etc.

```
nmap -A [target host]
```

***

## OS Discovery

Provide information about open ports, service running, name of OSrunningon target sytem.

```
nmap -A [target host]
```

***

## NSE Scripts

Display target OS, computer name, NetBIOS computer name, etc.

```
nmap –script smb-os-discovery.nse [target host]
```

