# DNS

Perform DNS enumeration that will process yield information such as DNS server names, hostnames, machine names, usernames, IP Addresses, and aliases assigned within a target domain.

DNS Enumeration is technique used to obtain information about the DNS servers and network infrastructure of the target organization. DNS enumeration can be performed using following technique:&#x20;

* Zone transfer&#x20;
* DNS cache snooping&#x20;
* DNSSEC zone walking.

## Zone Transfer

DNS Zone Transfer is the process of transferring a copy of the DNS zone file from the primary DNS server to a secondary DNS server. Most cases, DNS server maintains a spare or secondary server for redundancy, which holds all information stored in the main server. If the DNS transfer setting is enabled on the target DNS server, it will give DNS information; if not, it will return an error saying it has failed or refuses the zone transfer.&#x20;

Perform DNS enumeration through zone transfer by using **dig** (Linux) and **nslookup** (Windows) utilities.

### dig

#### Retrieve Information about all the DNS name servers

**dig** could retrieve info about target host addresses, nameservers, email exchanges, etc.

```
dig ns [target domain]
```

#### Retrieve Zone Information

```
dig @[name server] [target domain] axfr
```

After retrieving DNS name server information, attacker can use one of the servers to test whether the target DNS allow zone transfer or not. A pentester should attempt DNSZoneTransfer on different domains on the target organization.

### nslookup

This will resolve the target domain information. SOA mean Start of Authority. The result displaying information about the target domain such as primary name server and responsible mail addr.

```
nslookup
set querytype=soa
[domain]
```

#### Request a zone transfer of specified name server.

```
ls -d [name server]
```

After retrieving DNS name server information, attacker can use one of the servers to test whether the target DNS allow zone transfer or not. A pentester should attempt DNSZoneTransfer on different domains on the target organization.

***

## DNSSEC Zone Walking

DNSSEC Zone Walking is a DNS enumeration technique that is used to obtain the internal records of the target DNS server if the DNS zone is not properly configured. Enumeration of zone information can assist in building a host network map. There are various DNSSEC zone walking tools that can be used to enumerate the target domain’s DNS record files.&#x20;

Use **DNSRecon** tool to perform DNS enumeration through DNSSEC Zone Walking.

{% embed url="https://github.com/darkoperator/dnsrecon" %}

### Display enumerated DNS records for the target domain.

```
./dnsrecon.py -d [target domain] -z
```

> -d: specifies the target domain
>
> &#x20;-z: specifies that the DNSSEC zone walk be performed with standard enumeration

Using DNSRecon tool, the attacker can enumerate general DNS records for a given domain (MX, SOA, NS, A,AAAA, SPF, and TXT). These DNS records contain digital signatures based on public-key cryptography to strengthen authentication in DNS.&#x20;

Other DNSSEC Zone Walking enumerators:&#x20;

* LDNS&#x20;
* Nsec3map&#x20;
* Nsec3walker&#x20;
* DNSwalk

***

## DNS Enumeration with nmap

Nmap can be used for scanning domains and obtaining a list of subdomains, records, IPaddresses, and other valuable information from the target host. Use Nmap to performDNS enumeration on the target system.

### Display list of all available DNS Service

```
nmap --script=broadcast-dns-service-discovery [target domain]
```

### Display list of all the Subdomains associated&#x20;

```
nmap -T4 -p 53 --script dns-brute [target domain]
```

### Display Various Common Service (SRV) records for a given domain name

```
nmap --script dns-srv-enum --script-args "dns-srv-enum.domain='[target domain]'"
```

Using this information, attackers can launch web application attacks such as injection attacks, brute-force attacks, and DoS attacks on the target domain.
