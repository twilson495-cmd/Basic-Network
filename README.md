# Packet Tracer Homelab: Two-Switch Router Network

Just a basic network I built in Packet Tracer to practice router configuration and connecting different subnets.

## What's in it

- 1 Router (Cisco 2911)
- 2 Switches (Cisco 2960-24TT)
- 4 PCs (2 per switch)

Router1 connects to both switches. Switch1 has PC0 and PC1, Switch2 has PC2 and PC3.

- Switch1 side: 192.168.1.0/24
- Switch2 side: 192.168.2.0/24

## Steps

### 1. Set up the topology
Placed the router, switches, and PCs first before cabling anything to the router.

![Initial topology](images/Screenshot%202026-09-06%20235519.png)

### 2. Configured the first router interface
Connected everything, then went into the router and set up Gi0/0:

Router(config)#interface gigabitethernet 0/0
Router(config-if)#ip add 192.168.1.1 255.255.255.0
Router(config-if)#no sh


Interface came up fine after that.

![Router Gi0/0 config](images/Screenshot%202026-09-07%20162825.png)

### 3. Configured the second router interface
Did the same thing for Gi0/1 so the other switch's subnet has a gateway too:

Router(config-if)#interface gigabitethernet 0/1
Router(config-if)#ip add 192.168.2.1 255.255.255.0
Router(config-if)#no sh


![Router Gi0/1 config](images/Screenshot%202026-09-07%20163003.png)

### 4. Set a static IP on a PC
Gave one of the PCs a static IP so it could reach the router:

- IP: 192.168.1.3
- Subnet Mask: 255.255.255.0
- Gateway: 192.168.1.1

![PC IP configuration](images/Screenshot%202026-09-07%20165927.png)

### 5. Tested it with a ping
Pinged another PC on the same subnet to make sure everything actually worked. First couple pings failed while things were settling, then it cleaned up and started replying fine.

![Ping test](images/Screenshot%202026-09-07%20170049.png)

## Notes

- Each subnet just gets its own router interface here, no subinterfaces or trunking.
- Still need to set static IPs on the PCs on the other side (192.168.2.0/24) with gateway 192.168.2.1 if I want full connectivity across both subnets.
