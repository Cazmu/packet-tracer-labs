Designed and configured a small corporate office network in Packet Tracer, using:

A Cisco Catalyst 3650 switch (Layer 3 capable)

A Cisco 2811 router (for VoIP Call Manager Express, CME)

11 PCs (user devices)

11 IP Phones (VoIP endpoints, one per office/PC)

3 Printers (shared devices)

The topology followed a star design — everything connects back to the central switch.

<img width="1203" height="704" alt="image" src="https://github.com/user-attachments/assets/5021af37-19ef-4b75-beed-4cbbe23259b2" />

<img width="1333" height="1053" alt="image" src="https://github.com/user-attachments/assets/d4675895-fa14-46fc-8684-d7dd0f2d3b38" />

Segmented traffic with VLANs to separate devices by role:

VLAN 10 → PCs (192.168.10.0/24, gateway 192.168.10.1)

VLAN 20 → IP Phones (192.168.20.0/24, gateway 192.168.20.1)

VLAN 30 → Printers (192.168.30.0/24, gateway 192.168.30.1)

(Optional) VLAN 60 → Native VLAN for trunks

Each VLAN got its own subnet and default gateway.

DHCP was configured so devices could auto-assign addresses:

Excluded gateway ranges (e.g., .1–.9).

One pool per VLAN (PCs, Phones, Printers).

Added Option 150 in VLAN 20 DHCP scope, pointing to the CME router (192.168.20.2), so phones know where to register.

```
ip dhcp excluded-address 192.168.10.1 192.168.10.9
ip dhcp excluded-address 192.168.20.1 192.168.20.9
ip dhcp excluded-address 192.168.30.1 192.168.30.9

ip dhcp pool VLAN10-PCs
 network 192.168.10.0 255.255.255.0
 default-router 192.168.10.1
 dns-server 8.8.8.8

ip dhcp pool VLAN20-Voice
 network 192.168.20.0 255.255.255.0
 default-router 192.168.20.1
 option 150 ip 192.168.20.2   ! Points phones to CME

ip dhcp pool VLAN30-Printers
 network 192.168.30.0 255.255.255.0
 default-router 192.168.30.1
 dns-server 8.8.8.8
```

VoIP Configuration (Router CME)

The Cisco 2811 router was configured with Call Manager Express to register phones and assign extensions(101/102/103)

```
telephony-service
 max-ephones 20
 max-dn 20
 ip source-address 192.168.20.2 port 2000
 create cnf-files
```

Printers were placed in VLAN 30.

They pulled DHCP leases automatically (e.g., 192.168.30.10–12).

Default gateway: 192.168.30.1.

This ensures isolation from user PCs while maintaining network access.

PCs (VLAN 10): Confirmed DHCP leases and connectivity to gateway (192.168.10.1).

Phones (VLAN 20): Registered to CME, displayed assigned extensions (101–111).

Printers (VLAN 30): Pulled DHCP correctly and responded to pings.

VoIP Calls: Verified phones could successfully dial each other by extension (e.g., 101 → 102).

Inter-VLAN Routing: PCs could reach printers, but VLAN segmentation ensured isolation between traffic types.

The project successfully implemented a segmented multi-VLAN office network with integrated VoIP and printing services.

DHCP worked across all VLANs.

Phones auto-registered with extensions via CME.

Printers and PCs functioned in their own networks.

Inter-VLAN routing and isolation requirements were met.

