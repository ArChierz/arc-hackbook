# 🪟 wevutil

1. Open cmd and run as administrator.  To display a list of event logs. (el : event-logs)

```
wevutil el
```

2. clear system logs (cl: clear-logs)

```
wevutil cl [log name]   # here, the log name is `system` to clear the system logs
```

Similarly, you can also clear application and security logs by issuing the same command with different log names (application, security). wevtutil is a CMD utility used to retrieve information about event logs and publishers. You can also use this CMD to install and uninstall event manifest, run queries, and export, archive, and clear logs.
