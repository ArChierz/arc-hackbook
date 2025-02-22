# Detect Network Sniffing

This section helps to understand possible defensive techniques used to defend a target network against sniffing attacks.

A professional EH or pentester should be able to detect network sniffing in the network. A sniffer on a network only captures data and runs in promiscuous mode, so it is not easy to detect. Promiscuous mode allows a network device to intercept and read each network packet that arrives in its entirety. The sniffer leaves no trace, since it doesn’t transmit data. Therefore, to detect sniffing attempts, you must use the various network sniffing detection techniques and tools discussed in this lab.

## Overview of Detecting Network Sniffing

Network sniffing involves using sniffer tools that enable the real-time monitoring and analysis of data packets flowing over computer networks. These network sniffers can be detected by using various techniques such as:

* **Ping Method**: Identifies if a system on the network is running in promiscuous mode
* **DNS Method**: Identifies sniffers in the network by analyzing the increase in network traffic
* **ARP Method**: Sends a non-broadcast ARP to all nodes in the network; a node on the network running in promiscuous mode will cache the local ARP address.
