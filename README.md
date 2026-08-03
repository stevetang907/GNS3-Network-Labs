# Lab 10 - Internet Connectivity with NAT/PAT and OSPF Default Route Advertisement

## Objective

The objective of this lab was to configure an enterprise edge router that provides Internet access to multiple internal networks using Network Address Translation (NAT/PAT). OpenWrt's firewall was configured to perform masquerading while FRRouting (FRR) advertised a default route through OSPF, allowing all routers and hosts to reach external networks without manually configured default routes.

---

## Network Topology

<p align="center">
  <img width="761" height="418" alt="image" src="https://github.com/user-attachments/assets/4463c601-b1fd-4bdd-bd41-0e81c7fc96b8" />
</p>

---

## Network Overview

| Device | Interface | IP Address | Purpose |
|---------|-----------|------------|---------|
| Router 1 | br-lan | 192.168.10.1/24 | LAN |
| Router 1 | eth1 | 10.0.12.1/30 | Transit to Router 2 |
| Router 1 | eth2 | DHCP (192.168.2.x) | Internet/WAN |
| Router 2 | br-lan | 192.168.20.1/24 | LAN |
| Router 2 | eth1 | 10.0.12.2/30 | Transit to Router 1 |
| Router 2 | eth2 | 10.0.23.1/30 | Transit to Router 3 |
| Router 3 | br-lan | 192.168.30.1/24 | LAN |
| Router 3 | eth1 | 10.0.23.2/30 | Transit to Router 2 |
| PC1 | NIC | 192.168.10.10/24 | Host |
| PC2 | NIC | 192.168.20.10/24 | Host |
| PC3 | NIC | 192.168.30.10/24 | Host |

---

## Lab Tasks

This lab consisted of the following major tasks:

- Configured Router 1 as the enterprise edge router.
- Connected Router 1 to the GNS3 NAT node.
- Configured the WAN interface using DHCP.
- Verified Internet connectivity.
- Configured OpenWrt firewall zones.
- Enabled NAT/PAT (Masquerading).
- Advertised the default route through OSPF.
- Verified Internet connectivity from every router and host.

---

## WAN Configuration

Router 1 was configured to obtain its WAN address automatically from the GNS3 NAT node.

```text
config interface 'wan'
    option device 'eth2'
    option proto 'dhcp'
```

After restarting the network service, Router 1 received:

- WAN IP Address
- Default Gateway
- DNS Server

Internet connectivity was verified by successfully pinging both an IP address and a domain name.

---

## Firewall Configuration

OpenWrt's firewall was configured using two security zones.

### LAN Zone

- Input: ACCEPT
- Output: ACCEPT
- Forward: ACCEPT

### WAN Zone

- Input: REJECT
- Output: ACCEPT
- Forward: DROP
- Masquerading Enabled

Traffic from the LAN zone was forwarded to the WAN zone while source NAT translated all internal private addresses into Router 1's WAN address.

The transit interface was also added to the trusted LAN firewall zone so that traffic arriving from Routers 2 and 3 could be forwarded toward the WAN interface.

---

## OSPF Default Route Advertisement

Router 1 was configured to advertise its default route into Area 0.

```text
router ospf
 default-information originate
```

Routers 2 and 3 automatically learned the default route through OSPF without requiring manually configured static routes.

---

## Verification

### Router 1 Internet Connectivity

Router 1 successfully reached external hosts.

<p align="center">
  <img width="491" height="471" alt="image" src="https://github.com/user-attachments/assets/f8d1a2fe-8f75-4ff1-9e88-6affc417e6bc" />
</p>

---

### OSPF Default Route

Router 2 and Router 3 successfully learned the default route through OSPF.

#### Router 2:
<p align="center">
  <img width="634" height="415" alt="image" src="https://github.com/user-attachments/assets/3fa66d66-6cb6-47e8-8566-2c96e41e2428" />
</p>

#### Router 3:
<p align="center">
  <img width="635" height="383" alt="image" src="https://github.com/user-attachments/assets/7b3c19c5-17f1-4e73-ba6e-21079bafd9bc" />
</p>

---

### End-to-End Internet Connectivity

All routers and hosts successfully reached the Internet through Router 1.

| Device | Test | Result |
|---------|------|--------|
| Router 1 | Ping 8.8.8.8 | ✅ Success |
| Router 2 | Ping 8.8.8.8 | ✅ Success |
| Router 3 | Ping 8.8.8.8 | ✅ Success |
| PC1 | Ping 8.8.8.8 | ✅ Success |
| PC2 | Ping 8.8.8.8 | ✅ Success |
| PC3 | Ping 8.8.8.8 | ✅ Success |

<p align="center">
  <img width="448" height="101" alt="image" src="https://github.com/user-attachments/assets/15490d5f-dc14-497d-8b15-842c9d8c6578" />
</p>

---

## Challenges Encountered

- The temporary WAN interface initially used the name `tempwan`, which prevented the firewall's WAN zone from applying correctly.
- The WAN interface was renamed to `wan` to match the firewall configuration.
- Router 1's transit interface needed to be added to the LAN firewall zone so traffic from downstream routers could be forwarded to the WAN interface.

These troubleshooting steps reinforced the relationship between routing protocols, firewall policies, and Network Address Translation.

---

## What I Learned

Through this lab I learned how to:

- Configure an enterprise edge router.
- Configure a DHCP WAN interface.
- Configure OpenWrt firewall zones.
- Enable Network Address Translation (NAT/PAT).
- Configure source NAT using masquerading.
- Advertise a default route using OSPF.
- Verify dynamic default route propagation.
- Troubleshoot firewall forwarding issues.
- Verify end-to-end Internet connectivity across multiple routed networks.

---

## Key Takeaways

This lab combined several networking concepts into a single enterprise-style deployment.

Rather than relying on manually configured default routes or multiple Internet connections, Router 1 served as the organization's single Internet gateway while OSPF dynamically distributed the default route throughout the network. OpenWrt's firewall performed NAT/PAT, allowing all internal private networks to share one public-facing connection.

This lab demonstrates how dynamic routing, firewall policies, and Network Address Translation work together to provide secure Internet access in an enterprise network.

---

## Skills Demonstrated

- OpenWrt Administration
- FRRouting (FRR)
- OSPF Dynamic Routing
- Default Route Advertisement
- NAT/PAT (Masquerading)
- DHCP WAN Configuration
- Firewall Zone Configuration
- Enterprise Edge Router Deployment
- Network Troubleshooting
- End-to-End Connectivity Testing
