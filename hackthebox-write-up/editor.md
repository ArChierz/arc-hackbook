# Editor

**Machine Name**: Editor

**Machine OS**: Linux

**Difficulty**: Easy

## Introduction



## Information Gathering



### TCP Scan



```
nmap -sT -T4 10.10.11.87 -vvv 
```



```
PORT      STATE     SERVICE         REASON

```



## Enumeration



## Gaining Access



## Privilege Escalation



## Clearing Logs

to clear the logs in linux machine, simply use the history command.

```
history -c
```

This only effective if the .bash\_history set to **/dev/null.**

## Remediation

To remediate the following issue, do the following action:

## Reference

{% embed url="https://github.com/peass-ng/PEASS-ng/tree/master/linPEAS" %}
