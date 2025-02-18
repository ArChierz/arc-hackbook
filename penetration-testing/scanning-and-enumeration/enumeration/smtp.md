# SMTP

to obtain a list of valid users, delivery addresses, message recipients on an SMTP server.

Simple Mail Transfer Protocol (SMTP) is an internet standard based communication protocol for electronic mail transmission. Mail systems commonly use SMTP with POP3 and IMAP, enable users to save messages in the server mailbox and download them from the server when necessary, SMTP uses mail exchange (MX) servers to direct mail via DNS. Run on TCP port 25, 2525, 587.

## nmap

### Display a list of all possible mail users on the target

```
nmap -p 25 -script=smtp-enum-users [target]
```

### Display a list of open SMTP relays on the target

```
nmap -p 25 --script=smtp-open-relay [target]
```

### Display list all SMTP commands available

```
nmap -p 25 --script=smtp-commands [target]
```

Using this information, the attackers can perform password spraying attacks to gain unauthorized access to the user accounts.

