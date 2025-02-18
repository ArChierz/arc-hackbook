# SNMP

Carry out SNMP enumeration to extract information about network resources (hosts, routers, devices, and shares) and network information (such as ARP tables, routing tables, device-specific information, and traffic statistics). With this information, could do further scan target for underlying vulnerabilities, build a hacking strategy, and launch attacks.

SNMP (Simple Network Management Protocol) is an application layer protocol that runs on UDP and maintains and manages routers, hubs, and switches on an IP network. The agents run on networking devices on Windows and UNIX networks. SNMP enumeration uses SNMP to create a list of the user accounts and devices on a target computer. SNMP employs two types of software components for communication: SNMP agent and SNMP management station. SNMP agent located in networking device, while SNMP management station communicates with the agent.

***

## snmp-check

This tool will enumerates the target machine, listing sensitive information such as System Information and User Accounts. It also include Network Information, Network Interfaces, Network IP, Routing Information, TCP connections and listening ports, listening UDP ports, Network services, Processes running, File system information, device information, storage information, software components, shares, and IIS server information.

```
snmp-check [IP target]
```

***

## SoftPerfect Network Scanner

SoftPerfect Network Scanner can ping computers, scan ports, discover shared folders, and retrieve practically any information about network devices via WMI (Windows Management Instrumentation), SNMP, HTTP, SSH, and PowerShell. The program also scans for remote services, registries, files, and performance counters. It can check for a user-defined port and report if one is open, and is able to resolve hostnames, as well as auto detect your local and external IP range. Offer flexibility filtering and display options, and can export the NetScan results to a variety of formats, from XML to JSON. It supports remote shutdown and Wake-On-LAN.

***

## SNMPWalk

Command line tool that scan numerous SNMP nodes instantly and identifies a set of variables that are available for accessing the target network. Issued to root node so that the information from all the sub nodes such as routers and switches can be fetched.



The output displays all the OIDs, variables, and other associated information.

```
snmpwalk -v1 –c public [target]
```

> -v: specifies SNMP version number&#x20;
>
> -c: sets a community string



Displays data transmitted from the SNMP agent to the SNMP server, including information on server, user credentials, and other parameters.

```
snmpwalk -v2c -c public [target]
```

> -v: specifies SNMP version number&#x20;
>
> -c: sets a community string

***

## nmap

Use the Nmap script against an SNMP remote server to retrieve information related to the hosted SNMP services.

Displaying information regarding **SNMP server type and operating system details**.

```
nmap -sU -p 161 --script=snmp-sysdescr [target]
```

> -sU: UDP scan&#x20;
>
> -p: specifies port to be scanned &#x20;
>
> \--script: argument used to execute a given script



Displaying information of **list all the running SNMP processes along with associated ports on the target machine**.

```
nmap -sU -p 161 --script=snmp-processes [target]
```

> -sU: UDP scan&#x20;
>
> -p: specifies port to be scanned&#x20;
>
> \--script: argument used to execute a given script



Displaying information of **list all the applications running on the target machine**.

```
nmap -sU -p 161 --script=snmp-win32-software [target]
```

> -sU: UDP scan&#x20;
>
> -p: specifies port to be scanned&#x20;
>
> \--script: argument used to execute a given script



Displaying information about the **Operating System, Network Interfaces, and applications that are installed on the target machine.**

```
nmap -sU -p 161 --script=snmp-interfaces [target]
```

> -sU: UDP scan&#x20;
>
> -p: specifies port to be scanned&#x20;
>
> \--script: argument used to execute a given script





***

## Other Tools

* Network Performance Monitor,
* OpUtils,&#x20;
* PRTG,&#x20;
* Network Monitor,&#x20;
* Engineer’s Toolset.

