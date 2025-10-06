# **Sumamry**

<img width="1665" height="1053" alt="image" src="https://github.com/user-attachments/assets/6e1de996-7273-46a5-95be-cb6b79605c1e" />

This project implements a three-floor enterprise network for a modern hotel using Cisco Packet Tracer. The design follows hierarchical networking principles to ensure scalability, security, and reliability across departments. The implementation includes VLAN segmentation, inter-VLAN routing, OSPF dynamic routing, DHCP services, and secure SSH management across all routers.

# **Network Topology Summary**
- Routers: 3 (one per floor, interconnect via serial DCE cables)
- Switches: 3 (one per floor)
- WAP(Wireless Access Points): 3 (One per floor for Mobile devices and Laptops)
- Printers: 1 per department
- Cabling: Ethernet for end devices, Serial DCE between routers
All routers are located in the IT department server room.
<img width="257" height="887" alt="image" src="https://github.com/user-attachments/assets/b4441133-c66e-498e-bab1-1e2224849209" />

# **Inter-Router Network Design**

Link    | Network       | Subnet Mask     
R1 <-> R2 | 10.10.10.0/30 | 255.255.255.252 

R2 <-> R3 | 10.10.10.4/30 | 255.255.255.252 

R1 <-> R3 | 10.10.10.8/30 | 255.255.255.252 

# **VLAN and Department Addressing**

Floor	Department | VLAN ID	| Subnet | Default Gateway

<img width="643" height="539" alt="image" src="https://github.com/user-attachments/assets/5fbd31d8-9150-4a54-a972-929f00c569ae" />

**1st Floor**

Reception         | 80      | 192.168.8.0/24 | 192.168.8.1     
Store             | 70      | 192.168.7.0/24 | 192.168.7.1     
Logistics         | 60      | 192.168.6.0/24 | 192.168.6.1

<img width="554" height="529" alt="image" src="https://github.com/user-attachments/assets/9cb39356-3f5f-477d-9fd6-bb633188c5ca" />
     
**2nd Floor**

Finance             | 50      | 192.168.5.0/24 | 192.168.5.1    
Human Resources     | 40      | 192.168.4.0/24 | 192.168.4.1     
Sales & Marketing   | 30      | 192.168.3.0/24 | 192.168.3.1

<img width="765" height="466" alt="image" src="https://github.com/user-attachments/assets/e940c220-ba39-479b-9571-51d829c50bf8" />

**3rd Floor** 

Admin             | 20      | 192.168.2.0/24 | 192.168.2.1     
IT                | 10      | 192.168.1.0/24 | 192.168.1.1     

Each VLAN corresponds to a department, providing logical isolation while maintaining communication through inter-VLAN routing.

# **Configurations**

**Routing**
- Dynamic Routing Protocol: OSPF (Area 0 backbone)
- Each router advertises:
  - Its connected VLAN subnets
  - the /30 serial links
- Ensures full connectivity across all floors and departments

# **DHCP**
- DHCP pools configured on routers for each VLAN subnet.

- End devices automatically receive:
  - IP Address
  - Subnet Mask
  - Default Gateway
  - DNS Server (8.8.8.8)

# **Security and Management**
- SSH configured on all routers for secure remote access
- **Port Security** implemented on IT switch:
  - Port Fa0/1 (Test-PC) restricted to one MAC address.
  - Sticky MAC enabled
  - Violation mode: shutdown
 
# **Wireless**
- Each floor has a Wi-Fi Access Point configured to extend its respective VLAN network for mobile devices and laptops.

# **End Devices**
- Each department inclues:
  - 3 PCs
  - 1 Printer
  - WAP
- IT department has the Test-PC for SSH and security validation

# **Tools and Devices Used**
- Cisco Packet Tracer 8.2.2.0400
- Cisco 2911 Routers
- Cisco 2960 Switches
- PCs, Laptops, Printers, Access Points

# **Conclusion**
This project demonstrates the design and implementation of a multi-floor, multi-department enterprise network. It ensures efficient communication, security, and centralized management using Cisco best practices and achieving full DHCP automation, OSPF dynamic routing, and VLAN-based segmentation.
