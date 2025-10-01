Designed and configured a small corporate office network in Packet Tracer, using:

A Cisco Catalyst 3650 switch (Layer 3 capable)

A Cisco 2811 router (for VoIP Call Manager Express, VLAN configuration, and firewall configuration)

6 PCs (user devices)

6 IP Phones (VoIP endpoints, one per PC)

3 Printers (shared devices)

<img width="1401" height="883" alt="image" src="https://github.com/user-attachments/assets/5ee617e9-5957-41b1-ba63-0f68b5d33b51" />

Segmented traffic with VLANs to separate devices by role:

VLAN 10 - PCs (192.168.10.0/24, gateway 192.168.10.1)

VLAN 20 - IP Phones (192.168.20.0/24, gateway 192.168.20.1)

VLAN 30 - Management (192.168.30.0/24, gateway 192.168.30.1)

VLAN 40 - MISC - Not configured 

VLAN 50 - NATIVE - Native VLAN for trunks

VLAN 60 - Printer (192.168.60.0/24, gateway 192.168.60.1)

Each VLAN has its own subnet and default gateway.

DHCP was configured so devices could auto-assign IP addresses:

Excluded gateway ranges (e.g., .1–.10).

Added Option 150 in VLAN 20 DHCP scope, pointing to the router (192.168.20.2), so phones know where to register.

Firewall configurations implemented
