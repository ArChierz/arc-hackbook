# NFS

Perform NFS enumeration to identify exported directories and extract a list of clients connected to the server, along with their IP addresses and shared data associated with them. This information enable to spoof target IP addresses to gain full access to the shared files on the server.

Port: 2049

## RPCScan

NFS (Network File System) is a type of file system that enables computer users to access, view, store, and update files over a remote server. This remote data can be accessed by the client computer in the same way that it is accessed on the local system. RPCScan communicates with RPC (Remote Procedure Call) services and check misconfigurations on NFS shares. It lists RPC services, mountpoints, and directories accessible via NFS. It can also recursively list.

```
python3 rpc-scan.py 10.10.1.19 --rpc
```

***

## SuperEnum

SuperEnum includes a script that performs a basic enumeration of any openport, including NFS port 2049.

{% embed url="https://github.com/p4pentest/SuperEnum" %}



