# Covering Tracks

To maintain access to the target system longer and avoid detection, they need to clear any traces of their intrusion. It is also essential to avoid a traceback and possible prosecution for hacking.

One of the primary techniques to achieve this goal is to manipulate, disable, or erase the system logs. Once you have access to the target system, you can use inbuilt system utilities to disable or tamper with the logging and auditing mechanisms in the target system.

To remain undetected, the intruders need to erase all evidence of security compromise from the system. To achieve this, they might modify or delete logs in the system using certain log-wiping utilities, thus removing all evidence of their presence.

Various techniques used to clear the evidence of security compromise are as follow:

* **Disable Auditing**: Disable the auditing features of the target system
* **Clearing Logs**: Clears and deletes the system log entries corresponding to security compromise activities
* **Manipulating Logs**: manipulating logs in such a way that an intruder will not be caught in illegal actions
* **Covering Tracks on the Network**: Use techniques such as reverse HTTP shells, reverse ICMP tunnels, DNS tunneling, and TCP parameters to cover tracks on the network.
* **Covering Tracks on the OS**: Use NTFS streams to hide and cover malicious files in the target system
* **Deleting Files**: Use CMD-tools such as Cipher.exe to delete the data and prevent its future recovery
* **Disabling Windows Functionality**: Disable Windows functionality such as last access timestamp, Hibernation, Virtual Memory, and System Restore Points to cover tracks
