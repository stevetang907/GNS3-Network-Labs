# Lab 9 - Dynamic Routing with OSPF

## Objective

The objective of this lab was to replace manually configured static routes with the Open Shortest Path First (OSPF) dynamic routing protocol using FRRouting (FRR) on OpenWrt routers. By completing this lab, I learned how routers automatically exchange routing information, build routing tables dynamically, and maintain end-to-end connectivity without manually configured routes.

---

## Network Topology

<img width="1277" height="796" alt="Screenshot 2026-08-01 152452" src="https://github.com/user-attachments/assets/31b46438-62f9-460d-a6bd-72e5fd2feee1" />

---

## IP Addressing

| Device | Interface | IP Address | Purpose |
|---------|-----------|------------|---------|
| Router 1 | br-lan | 192.168.10.1/24 | LAN |
| Router 1 | eth1 | 10.0.12.1/30 | Transit to Router 2 |
| Router 2 | br-lan | 192.168.20.1/24 | LAN |
| Router 2 | eth1 | 10.0.12.2/30 | Transit to Router 1 |
| Router 2 | eth2 | 10.0.23.1/30 | Transit to Router 3 |
| Router 3 | br-lan | 192.168.30.1/24 | LAN |
| Router 3 | eth1 | 10.0.23.2/30 | Transit to Router 2 |
| PC1 | NIC | 192.168.10.10/24 | VLAN/LAN Host |
| PC2 | NIC | 192.168.20.10/24 | VLAN/LAN Host |
| PC3 | NIC | 192.168.30.10/24 | VLAN/LAN Host |

---

## OSPF Configuration

### Router 1

```text
router ospf
 router-id 1.1.1.1
 network 192.168.10.0/24 area 0
 network 10.0.12.0/30 area 0
```

### Router 2

```text
router ospf
 router-id 2.2.2.2
 network 192.168.20.0/24 area 0
 network 10.0.12.0/30 area 0
 network 10.0.23.0/30 area 0
```

### Router 3

```text
router ospf
 router-id 3.3.3.3
 network 192.168.30.0/24 area 0
 network 10.0.23.0/30 area 0
```

---

## Verification

### OSPF Neighbor Relationships

The routers successfully formed OSPF adjacencies and entered the **FULL** state, allowing them to exchange routing information dynamically.

<img width="820" height="177" alt="Screenshot 2026-08-01 152535" src="https://github.com/user-attachments/assets/df65f0a2-5f55-401d-a334-f2fe454675b7" />

---

### OSPF Routing Table

The routing tables now contain routes learned through OSPF, identified by the **O** route code.

<img width="817" height="482" alt="Screenshot 2026-08-01 152542" src="https://github.com/user-attachments/assets/2be4e0d6-c3e1-4018-a5c3-22122159cc90" />

---

### OSPF Routing Table

<img width="412" height="543" alt="Screenshot 2026-08-01 152641" src="https://github.com/user-attachments/assets/a2e64915-7ecc-4c74-a602-607ec75a15fc" />

---

### End-to-End Connectivity

All PCs were able to successfully communicate across all three LANs without using static routes.

Successful tests included:

- PC1 → PC2
- PC1 → PC3
- PC2 → PC1
- PC2 → PC3
- PC3 → PC1
- PC3 → PC2

<img width="612" height="267" alt="Screenshot 2026-08-01 152630" src="https://github.com/user-attachments/assets/699ad7e6-9819-4abb-94ef-e456fd26dd78" />

---

## Challenges Encountered

During this lab, I encountered several challenges while configuring FRRouting on OpenWrt 25.12:

- The OSPF daemon (`ospfd`) is disabled by default after installation and had to be enabled in `/etc/frr/daemons`.
- FRRouting services needed to be restarted before the OSPF daemon became available in `vtysh`.
- Temporary Internet connectivity through a GNS3 NAT node was required to download the FRR packages.

These issues provided valuable experience troubleshooting routing software installation and service management on Linux-based network devices.

---

## What I Learned

Through this lab I learned how to:

- Install and configure FRRouting (FRR) on OpenWrt.
- Enable and manage FRR routing daemons.
- Configure OSPF router IDs.
- Advertise connected networks into OSPF Area 0.
- Verify OSPF neighbor adjacencies.
- Verify dynamically learned routes.
- Troubleshoot OSPF daemon startup issues.
- Compare dynamic routing with manually configured static routing.

---

## Key Takeaways

Static routing works well for small networks but quickly becomes difficult to maintain as networks grow. OSPF automates route discovery and updates, making enterprise networks easier to manage, more scalable, and more resilient to topology changes.

This lab represents the transition from manually managed routing to enterprise dynamic routing protocols and serves as a foundation for future labs involving ACLs, NAT/PAT, and larger enterprise network designs.
