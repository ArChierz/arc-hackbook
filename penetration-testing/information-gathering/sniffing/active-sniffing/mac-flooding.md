# MAC Flooding

**MAC Flooding** is a technique used to compromise the security of network switches that connect network segments or network devices. Attackers use the MAC flooding technique to force a switch to act as a hub, so they can easily sniff the traffic.

***

## Macof

**Macof** is a Unix and Linux tool that is a part of the dsniff collection. It floods the local network with random MAC addresses and IP addresses, causing some switches to fail and open in repeating mode, thereby facilitating sniffing. This tool floods the switch’s CAM tables (131.000 per minute) by sending forged MAC entries. When the MAC table fills up, the switch converts to a hub-like operation where an attacker can monitor the data being broadcast.

### PoC

1. Set up a network listener with Wireshark
2. In attacker, set up **macof** by following code:

```
macof -i eth0 -n 10
```

Can also target a single system:

```
macof -i eth0 -d [target ip address]
```

3. Observe that there are many packets from random IP address with protocol **IPv4**, check them in **Ethernet II** node, will display the MAC address.&#x20;

**macof** sends the packet with random MAC and IP address to all active machines in the local network, if using multiple target will observe the same packets on all target machine.



&#x20;
