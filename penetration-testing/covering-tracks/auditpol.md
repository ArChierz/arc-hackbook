# 🪟 auditpol

**Auditpol.exe** is the CMD-utility tool to change the Audit Security settings at the category and sub-category levels. You can use Auditpol to enable or disable security auditing on local or remote systems and to adjust the audit criteria for different categories of security events.

In real-time, the moment intruders gain administrative privileges, they disable auditing with the help of auditpol.exe. Once they complete their mission, they turn auditing back on by using the same tool **audit.exe**.

1. Open **cmd** and run as administrator.
2. To view all the audit policies, type:

```
auditpol /get /category:*
```

3. Enable the audit policies

```
auditpol /set/category:"system","account logon" /success:enable /failure:enable 
```

4. Check whether the audit policies are enabled with same code as no.2
5. To clear audit policies

```
auditpol /clear /y
```

6. Check whether the audit are cleared with same code as no.2

Now it all gone. No Auditing indicates that the system is not logging audit policies. For demonstration purposes, we are clearing logs on the same machine. In real-time, the attacker performs this process after gaining access to the target system to clear traces of their malicious activities from the target system.

