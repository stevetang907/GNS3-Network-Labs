# Lab 01 - Basic Switching
Objective

Create a simple switched network consisting of two hosts connected through an Ethernet switch and verify end-to-end connectivity.

Topology


Devices Used
1 Ethernet Switch
2 VPCS Hosts
1 NAT Device
IP Addressing
Device	IP Address
PC1	192.168.2.128
PC2	192.168.2.129
Gateway	192.168.2.1
Configuration
PC1

ip dhcp

PC2

ip dhcp

Verification
PC1 to PC2 Ping

[Insert screenshot]

Internet Connectivity

Ping to 8.8.8.8 successful.

[Insert screenshot]

Lessons Learned
Learned how DHCP assigns addresses.
Learned how switches forward traffic.
Learned how to verify connectivity using ping.
