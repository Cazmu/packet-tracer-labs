# **Advanced Enterprise Network Design**
**Design and Implementation of a Secure Company Network System**

**This project demonstrates the design and implementation of a secure, redundant, and scalable enterprise network for Cytonn Innovation Ltd, a cloud-driven organization with 600 employees.**
**The design integrates Cisco ASA firewalls, dual ISPs, hierarchical switching, and VLAN segmentation. It uses enterprise-grade protocols and technologies to provide strong security, high availability, and efficient communication.**

# **External and Edge Layer**

The external layer connects the enterprise network to two redundant ISPs for continuous Internet access and automatic failover.

Internet and ISP configuration:

- SEACOM ISP (Primary): 20.20.20.0/30 → 105.100.50.0/30 → ASA OUTSIDE1

- SAFARICOM ISP (Secondary): 30.30.30.0/30 → 197.200.100.0/30 → ASA OUTSIDE2

Internet test PCs:

- USA-INTERNET-USER: 8.0.0.10/8

- CHINA-INTERNET-USER: 8.0.0.20/8

Firewall default routes:
```
route OUTSIDE1 0.0.0.0 0.0.0.0 105.100.50.1 1
route OUTSIDE2 0.0.0.0 0.0.0.0 197.200.100.1 70
```

# **Firewall Security Zones (Cisco ASA 5506-X)**

Security zones:

- OUTSIDE1 and OUTSIDE2 – connected to SEACOM and SAFARICOM ISPs

- INSIDE1 and INSIDE2 – enterprise LAN segments

- DMZ – web, FTP, email, and DNS servers

Firewall configuration and features:

- Dynamic PAT for inside, WLAN, and DMZ outbound traffic

- Identity NAT (no-NAT) for internal DMZ communication

- ACLs and ICMP inspection for diagnostics and security filtering

- OSPF area 0 adjacency with core switches

- Syslog and SNMP for centralized monitoring

- Dual ISP failover for redundancy

# **Core and Distribution Layers**

The enterprise backbone is built on two Layer 3 core switches that manage routing, redundancy, and inter-VLAN communication.

Core layer features:

- OSPF area 0 for dynamic route exchange between ASA and switches

- HSRP for default gateway redundancy

- EtherChannel (LACP) for link aggregation and higher bandwidth

- Inter-VLAN routing for departmental communication

- Trunk ports connect to access-layer switches

Each core switch uplinks to Catalyst 2960 switches on each floor using trunk ports carrying VLANs 10, 20, 50, 70, and 199.

# **Server Room and DMZ**

Subnets and purpose:

- 10.11.11.0/27 – DMZ zone for public-facing servers

- 192.168.10.0/24 – management network for SSH, SNMP, and Syslog

- 10.20.0.0/16 – WLAN network for corporate Wi-Fi

- 10.11.11.32/27 – internal server network

DMZ services include:

- Web, FTP, Email, DNS, and NAS servers

- Active Directory, DHCP, and RADIUS

- Centralized authentication and time synchronization (NTP)

- Remote management through the management VLAN

# **Floor Access Layers (Cisco Catalyst 2960 Switches)**

Each floor has a Catalyst 2960 access switch that connects wired and wireless devices. All clients receive IP addresses via DHCP.

**First Floor – Sales and Marketing**

Switch Model: Catalyst 2960
Gateway: 172.16.0.1
IP Assignment: DHCP

VLAN configuration:

- VLAN 10: Management – 192.168.10.0/24

- VLAN 20: LAN – 172.16.0.0/16

- VLAN 50: WLAN – 10.20.0.0/16

- VLAN 70: VoIP – 172.30.0.0/16

- VLAN 199: Blackhole – unused ports

Connected devices:

- 2 PC (VLAN 20)

- 2 Printer (VLAN 20)

- 4 VoIP Phones (VLAN 70)

- 2 Lightweight Access Point (VLAN 50)

- 6 Wireless Clients (VLAN 50)

Configuration highlights:

- Port security with sticky MAC

- STP PortFast and BPDU Guard

- Trunk uplink to core switch carrying VLANs 10, 20, 50, 70, 199

**Second Floor – Finance and Administration**

Switch Model: Catalyst 2960
Gateway: 172.16.2.1
IP Assignment: DHCP

VLAN configuration:

- VLAN 10: Management – 192.168.10.0/24

- VLAN 20: LAN – 172.16.0.0/16

- VLAN 50: WLAN – 10.20.0.0/16

- VLAN 70: VoIP – 172.30.0.0/16

- VLAN 199: Blackhole

Connected devices:

- 2 PCs

- 2 Printer

- 4 IP Phones

- 2 LAP

- 6 Wireless Clients

Additional configuration:

- DHCP from core DHCP server

- STP protection enabled for access ports

**Third Floor – ICT and Server Operations**

Switch Model: Catalyst 2960
Gateway: 172.16.6.1
IP Assignment: DHCP and Static (for servers)

VLAN configuration:

- VLAN 10: Management – 192.168.10.0/24

- VLAN 20: LAN – 172.16.0.0/16

- VLAN 50: WLAN – 10.20.0.0/16

- VLAN 70: VoIP – 172.30.0.0/16

- VLAN 199: Blackhole

Connected devices:

- 1 PCs

- 1 Printer

- 1 VoIP Phone

- 1 LAP

- 3 Wireless Clients

- 3 Inside Servers (10.11.11.32/27) hosting DHCP, DNS, AD, and RADIUS

Configuration details:

- Static IPs for all servers

- VLAN 10 for SSH, SNMP, and management access

- Trunk uplink to core switch for all VLANs

# **Network Protocols Implemented**

- OSPF – Dynamic routing between ASA and core switches

- HSRP – Default gateway redundancy between core switches

- LACP – EtherChannel bundling for bandwidth and fault tolerance

- VLANs and Inter-VLAN Routing – Logical segmentation for departments

- STP PortFast and BPDU Guard – Loop prevention and faster convergence

- DHCP and DHCP Relay – Centralized address management

- NAT (Dynamic and Identity) – Address translation for Internet and DMZ access

- ACLs – Access control between zones

- AAA (RADIUS) – Centralized authentication for administrators

- SNMP v3 – Real-time device monitoring

- Syslog – Centralized logging for network events

- NTP – Time synchronization across devices

- DNS – Internal and external name resolution

- QoS – Prioritization for voice and management traffic

- ICMP Inspection – Ping diagnostics through the ASA

- SSH and HTTPS – Secure management access

# **IP Address Summary**

- 192.168.10.0/24 – Management VLAN

- 172.16.0.0/16 – LAN (Departments)

- 10.20.0.0/16 – WLAN

- 172.30.0.0/16 – VoIP

- 10.11.11.0/27 – DMZ (Public Servers)

- 10.11.11.32/27 – Inside Server Network

- 105.100.50.0/30 – SEACOM ISP (Primary Link)

- 197.200.100.0/30 – SAFARICOM ISP (Secondary Link)

# **Testing and Validation**

- Verified OSPF adjacency between ASA and core switches

- Confirmed HSRP failover between redundant gateways

- Tested dual ISP failover routes

- Validated NAT and ACL configurations for secure Internet access

- Confirmed ICMP and HTTP communication between LAN, DMZ, and Internet

- Tested WLAN and VoIP connectivity between departments

- Monitored SNMP traps and Syslog logs for network visibility

# **Conclusion**

This enterprise network was designed to provide scalability, security, and redundancy for Cytonn Innovation Ltd.
By integrating multiple Cisco technologies including ASA firewalls, dual ISPs, dynamic routing (OSPF), gateway redundancy (HSRP), link aggregation (LACP), and VLAN segmentation, the network ensures:

- High availability and fault tolerance

- Strong perimeter and internal security

- Efficient traffic management

- Centralized monitoring and administration

This project represents a complete enterprise-grade deployment demonstrating the integration of routing, switching, wireless, VoIP, and firewall systems within a unified and secure infrastructure.
