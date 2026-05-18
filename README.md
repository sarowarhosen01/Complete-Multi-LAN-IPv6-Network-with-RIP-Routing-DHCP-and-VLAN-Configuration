# 04 IPv6 Complete Multi-LAN RIP Network with DHCP & VLAN Configuration

**Cisco Packet Tracer** project showcasing a fully functional IPv6-based enterprise network with VLAN segmentation, dynamic routing, and automated address assignment.

![Network Topology](topology-screenshot.png)  
*(Add a screenshot/export of your Packet Tracer topology here)*

## Project Overview

This lab implements a **multi-LAN IPv6 network** using:
- **VLANs** for logical segmentation (e.g., VLAN 10 - Admin, VLAN 20 - Sales, VLAN 30 - Engineering, etc.).
- **RIPng** (RIP for IPv6) for dynamic routing between LANs and routers.
- **DHCPv6** (or SLAAC + DHCPv6 options) for address assignment to clients in different VLANs.
- Inter-VLAN routing via router or Layer 3 switch.
- Full IPv6 addressing plan with proper subnetting.

The network supports end-to-end IPv6 communication, ping tests, web services (if simulated), and demonstrates real-world campus/branch network design principles.

## Network Topology Table

| Device Type       | Device Name     | Interfaces Used                  | Role                          | Connected To              |
|-------------------|-----------------|----------------------------------|-------------------------------|---------------------------|
| Router            | R1 (Core/Edge) | GigabitEthernet0/0, 0/1, etc.  | Main Router, RIPng, Inter-VLAN | R2, Switches             |
| Router            | R2             | Multiple GigabitEthernet        | Distribution Router           | R1, Access Switches      |
| Layer 2 Switch    | SW1 (Access)   | Fa0/1 - Fa0/24, Trunk ports     | VLAN Assignment               | PCs, Router              |
| Layer 2/3 Switch  | SW2 (Dist)     | Trunk + SVI                     | Inter-VLAN (if used)          | Other switches           |
| PCs / Laptops     | PC1, PC2, ...  | FastEthernet                    | End Devices (DHCP clients)    | Access Switches          |
| Server            | DHCP Server    | Ethernet                        | Centralized DHCP              | Core Switch/Router       |

*(Customize device names and connections based on your exact .pkt topology)*

**High-Level Topology:**
- Core Router(s) interconnected via point-to-point IPv6 links.
- Multiple Access Switches with VLAN-configured ports.
- Trunk links carrying multiple VLANs.
- RIPng enabled on all routers for automatic route advertisement.
- Clients in different VLANs receive IPv6 addresses via DHCP.

## IP Addressing Table (IPv6)

| VLAN / Subnet | Description          | IPv6 Prefix (Example)              | Default Gateway          | DHCP Pool Range                  |
|---------------|----------------------|------------------------------------|--------------------------|----------------------------------|
| VLAN 10      | Administration      | 2001:db8:1:10::/64                | 2001:db8:1:10::1        | 2001:db8:1:10::100 - ::1FF     |
| VLAN 20      | Sales / Users       | 2001:db8:1:20::/64                | 2001:db8:1:20::1        | 2001:db8:1:20::100 - ::1FF     |
| VLAN 30      | Engineering         | 2001:db8:1:30::/64                | 2001:db8:1:30::1        | 2001:db8:1:30::100 - ::1FF     |
| VLAN 99      | Management / Native | 2001:db8:1:99::/64                | 2001:db8:1:99::1        | N/A                              |
| Inter-Router | R1-R2 Link          | 2001:db8:1:100::/64               | -                        | -                                |

**Link-Local Addresses:** Automatically generated (`FE80::...`).  
**Unique Local (if used):** `FD00:...` for internal-only traffic.





Switch(config)# interface gi0/1
Switch(config-if)# switchport mode trunk
Switch(config-if)# switchport trunk allowed vlan 10,20,30
