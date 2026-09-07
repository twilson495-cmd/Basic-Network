# Packet Tracer Homelab: Two-Switch Router Network

A simple Cisco Packet Tracer lab demonstrating basic router configuration, static PC addressing, and inter-subnet connectivity across two switches.

## Topology

- 1x Router (Cisco 2911)
- 2x Switch (Cisco 2960-24TT)
- 4x PC (2 per switch)

Router1 connects to Switch1 and Switch2. Switch1 has PC0 and PC1 connected. Switch2 has PC2 and PC3 connected.

- Switch1 side: 192.168.1.0/24 (Gi0/0)
- Switch2 side: 192.168.2.0/24 (Gi0/1)

## Build Steps

### 1. Initial Topology
Placed one router and two switches, with two PCs connected to each switch. No cabling to the router yet.

![Initial topology](images/Screenshot%202026-09-06%20235519.png)

### 2. Router Configuration - Gi0/0
Cabled the router to both switches. Checked interface status with `show ip int b`, then configured GigabitEthernet0/0:

Router(config)#interface gigabitethernet 0/0
Router(config-if)#ip add 192.168.1.1 255.255.255.0
Router(config-if)#no sh


Confirmed the interface came up via %LINK-5-CHANGED and %LINEPROTO-5-UPDOWN messages.

![Router Gi0/0 config](images/Screenshot%202026-09-07%20162825.png)

### 3. Router Configuration - Gi0/1
Configured the second interface for the Switch2 subnet:

Router(config-if)#interface gigabitethernet 0/1
Router(config-if)#ip add 192.168.2.1 255.255.255.0
Router(config-if)#no sh


![Router Gi0/1 config](images/Screenshot%202026-09-07%20163003.png)

### 4. PC IP Configuration
Statically assigned an IP to a PC on the 192.168.1.0/24 subnet:

- IP Address: 192.168.1.3
- Subnet Mask: 255.255.255.0
- Default Gateway: 192.168.1.1

![PC IP configuration](images/Screenshot%202026-09-07%20165927.png)

### 5. Connectivity Test
Pinged 192.168.1.2 from a PC's command prompt to validate connectivity on the same subnet. Initial packet loss cleared up as ARP resolved, ending in a clean 0% loss with sub-millisecond round trip times.

![Ping test](images/Screenshot%202026-09-07%20170049.png)

## Notes

- This lab demonstrates basic dual-interface routing rather than subinterfaces/router-on-a-stick — each switch subnet has its own dedicated physical router interface.
- Repeat step 4 for PCs on the 192.168.2.0/24 side using gateway 192.168.2.1 for full end-to-end connectivity across both subnets.
