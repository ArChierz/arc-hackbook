# Various Information

## Information using Global Network Inventory

Global Network Inventory is used as an audit scanner in zero deployment and agent free environments. It scans single or multiple computers by IP range or domain, as defined by them host file. Use Global Network Inventory to enumerate various type of data from a target IP address range or single IP.

***

## Network Resource using Advanced IP Scanner

Advanced IP Scanner provides various types of information about the computers on a target network. The program shows all network devices, gives you access to shared folders, provides remote control of computers (via RDP and Radmin), and can even remotely switch computers off.&#x20;

Use Advanced IP Scanner to enum. The result displayed information about active hosts in the target network such as status, machine name, IP Address, Manufacturer name, and MAC addresses.

***

## Information form Windows and Samba host using Enum4Linux

Enum4Linux is a tool for enumerating information from Windows and Samba systems. It is used for shares enumeration, password policy retrieval, identification of remote OSes, detecting if hosts are in a workgroup or a domain, user listing on hosts, listing group membership information, etc. Enum4Linux to perform enumeration on a Windows and a Samba host.

### Display NetBIOS information under Nbstart Information

```
enum4linux -u [username] -p [password] -n [target]
```

> -n: disable NetBIOS name resolution. Speed up the scan

### Display Data such as Target Info, Workgroup/Domain, etc.

```
enum4linux -u [username] -p [password] -U [target]
```

> -U: retrieves the userlist

### Enumerate Target system and lists its OS details

```
enum4linux -u [username] -p [password] -o [target]
```

> -o: retrieves the OS information

### Display password policy information

```
enum4linux -u [username] -p [password] -P [target]
```

> -P: retrieves the password policy information

### Display Group Policy Information

```
enum4linux -u [username] -p [password] -G [target]
```

> -P: retrieves the group policy information

### Display Shared Folders

```
enum4linux -u [username] -p [password] -S [target]
```

> -S: retrieves sharelist

Using this information, attackers can gain unauthorized access to the user accounts andgroups, and view confidential information in the shared drives.
