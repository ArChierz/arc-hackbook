# NetBIOS

## Windows Command-line Utilities

nbtstat help in troubleshooting NETBIOS. This command removes and correctspreloaded entries using several case-sensitive switches. This can be used to enumerateinformation such as NetBIOS over TCP/IP protocol statistics, NetBIOS nametablelocal and remote computers, and NetBIOS name cache. Net use connects a computer to, or disconnects it from a shared resource.

```
nbtstat aa [IP address remote machine]
```

> -a: : display the NetBIOS name table of remote computer

```
nbtstat -c
```

> -c: list content cached

```
net use
```

> Display information about target such as connection status, shared folder/drive, andnetwork information. Attacker can read or write to a remote computer system, dependon availability of shares, or DoS.

***

## NetBIOS Enumerator

Tool that enables the use of remote network support and several other techniquessuch as SMB (server message block). It is used to enumerate details such as NetBIOSnames, usernames, domain names, and MAC address for a given range of IPaddress. If the debug window set to ready, then the scan is finished.

***

## nmap NSE Script

NSE allow users to write (and share) simple scripts to automate a wide varietyofnetworking tasks. Can be used for discovering NetBIOS shares on the network. Canretrieve the target’s NetBIOS names and MAC addresses. Moreover, increasing verbosity allows you to extract all names related to the system.

```
nmap -sV -v --script nbstat.nse [target IP address]
```

> -sV: detect the service versions&#x20;
>
> -v: enables the verbose output&#x20;
>
> \--script nbstat.nse: performs the NetBIOS enumeration.

```
nmap –sU –p 137 --script nbstat.nse [target IP address]
```

> -sU: Performs UDP scan&#x20;
>
> -p 137: scan port 137&#x20;
>
> \--script nbstat.nse: performs the NetBIOS enumeration.

***

## Other Tools

* Global Network Inventory&#x20;
* Advanced IP Scanner&#x20;
* Hyena&#x20;
* Nsauditor Network Security Auditor

