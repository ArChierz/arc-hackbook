# LDAP

Next step after perform SNMP enum is to perform LDAP enumeration to access directory listings within Active Directory or other directory services. Directory services provide hierarchically and logically structured information about the components of a network, from lists of printers to corporate email directories. Similar to company’s org chart. LDAP Enumeration allows to gather information about usernames, addresses, departmental details, server names, etc.

LDAP (Lightweight Directory Access Protocol) is an internet protocol for accessing distributed directory services over a network. LDAP uses DNS (Domain Name System) for quick lookups and fast resolution of queries. A client starts an LDAP session by connecting to a DSA (Directory System Agent), typically on TCP port 389, and sends an operation request to the DSA, which then responds. BER(Basic Encoding Rules) is used to transmit information between the client and server. One can anonymously query the LDAP service for sensitive information such as usernames, addresses, departmental details, and server names.

***

## Active Directory Explorer (AD Explorer)

Active Directory Explorer (AD Explorer) is an advanced Active Directory (AD) viewer and editor. It can be used to navigate an AD database easily, define favorite locations, view object properties and attributes without having to open dialog boxes, edit permissions, view an object’s schema, and execute sophisticated searches that can be saved and re-executed. Used AD Explorer to perform LDAP Enumeration on an AD domain and modify the domain user accounts.

You can modify other user profile attributes in the same way. You can also useotherLDAP enumeration tools such as Softerra LDAP Administrator, LDAP AdminTool, LDAP Account Manager, and LDAP Search.

***

## nmap

Using NSEscript can be used to perform queries to brute force LDAP authentication using the built-in username and password lists.

```
nmap -sU -p 389 [target]
```

```
nmap -p 389 --script ldap-brute --script-args ldap.base='"cn=users,dc=CEH,dc=com"' [target]
```

***

## Python

{% code title="ldap-enum.py" overflow="wrap" lineNumbers="true" %}
```python
import ldap3

server = ldap3.Server('10.10.1.22', get_info=ldap3.ALL, port=389)
connection = ldap3.Connection(server)
connection.bind()
server.info

connection.search(search_base='DC=CEH,DC=com', search_filter='(&(objectclass=*))', search_scope='SUBTREE', attributes='*')
connection.entries

connection.search(search_base='DC=CEH,DC=com', search_filter='(&(objectclass=person))', search_scope='SUBTREE', attributes='userpassword')
connection.entries
```
{% endcode %}

***

## ldapsearch

Ldapsearch is a shell-accessible interface to the ldap\_search\_ext(3) librarycall. Ldapsearch opens a connection to a LDAP server, binds the connection, and performsasearch using the specified parameters. The filter should comformto the stringrepresentation for search filters as defined in RFC 4515. If not provided, the default filterwill be objectClass=\*. Use ldapseacrh to perform LDAP enumeration on the target system.



### Gather details related to naming context

```
ldapsearch -h 10.10.1.22 -x -s base namingcontexts
```



### Obtain more information about the primary domain

```
ldapsearch -h 10.10.1.22 -x -b "DC=CEH,DC=com"
```



### Retrieve information related to all the objects in the directory tree

```
ldapsearch -h 10.10.1.22 -x -b "DC=CEH,DC=com" "objectclass=*"
```

