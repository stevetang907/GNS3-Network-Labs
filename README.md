# Lab 11 - OpenWrt Firewall Fundamentals

## Objective

The objective of this lab was to explore the OpenWrt firewall (firewall4) and understand how firewall zones, forwarding policies, NAT (Masquerading), and stateful packet inspection work within a multi-router enterprise network. The lab also demonstrated how firewall configuration interacts with dynamic routing protocols such as OSPF.

---

# Skills Demonstrated

- OpenWrt Firewall (firewall4)
- Firewall Zones
- Zone Forwarding
- nftables Inspection
- NAT/PAT (Masquerading)
- Stateful Firewall Concepts
- OSPF Integration
- Firewall Troubleshooting
- Enterprise Network Verification

---

# Network Topology

<img width="769" height="418" alt="image" src="https://github.com/user-attachments/assets/6169ccea-f338-4438-a701-0189e734cfaf" />

## Network Overview

| Device | Role |
|---------|------|
| Router 1 | Edge Router (Firewall, NAT/PAT, OSPF) |
| Router 2 | Distribution Router |
| Router 3 | Branch Router |
| PC1 | LAN 1 (192.168.10.0/24) |
| PC2 | LAN 2 (192.168.20.0/24) |
| PC3 | LAN 3 (192.168.30.0/24) |

---

# Part 1 - Firewall Zone Verification

## Router 1 Firewall Zones

<img width="382" height="64" alt="image" src="https://github.com/user-attachments/assets/cefa677e-2480-4e00-a89a-45dec0c929c4" />

### LAN Zone

**Interfaces included:**

lan and wan

---

## WAN Zone

<img width="340" height="143" alt="image" src="https://github.com/user-attachments/assets/bbe7e786-e3fe-40e2-9b5d-9dbb695e46ef" />

| Setting | Value |
|---------|-------|
| Input Policy |REJECT|
| Output Policy |ACCEPT|
| Forward Policy |DROP|
| Masquerading Enabled |1|

**Observations:**

The WAN zone is configured to reject unsolicited inbound traffic while allowing outbound traffic initiated by the router. Forwarding from the WAN to internal networks is dropped by default, providing protection against unauthorized access. Masquerading (NAT/PAT) is enabled so that devices on the private LANs can share Router 1's public-facing WAN IP address when accessing the Internet.

---

## Zone Forwarding

<img width="392" height="69" alt="image" src="https://github.com/user-attachments/assets/3959814a-4a04-441b-976e-b7397b016aec" />

### Question

**What does the LAN → WAN forwarding rule allow?**

It allows LAN users (VPCS) to communicate with the internet
---

# Part 2 - Inspecting the Active Firewall

## firewall4 Configuration

<img width="935" height="645" alt="image" src="https://github.com/user-attachments/assets/20184881-828b-4da4-a3da-3a3318048187" />


### Observations

What information can you identify from the generated firewall configuration?

The fw4 print output showed how OpenWrt translated the UCI firewall configuration into an active firewall policy. It identified the configured firewall zones (LAN and WAN), the default input, output, and forwarding policies for each zone, the forwarding rule allowing traffic from the LAN to the WAN, and the NAT (masquerading) rules used for Internet access. The output also showed the underlying nftables chains and rules that the firewall applies to inspect and filter network traffic.

---

## nftables Rules

<img width="948" height="652" alt="image" src="https://github.com/user-attachments/assets/23a63062-e01b-4069-996f-bb176f8e63c7" />

### Question

How does OpenWrt use nftables to enforce firewall policies?

OpenWrt uses firewall4 to automatically convert the firewall settings stored in the UCI configuration into nftables rules. Instead of manually writing nftables commands, the administrator defines firewall zones, forwarding rules, and NAT settings in /etc/config/firewall. Firewall4 then generates the appropriate nftables ruleset, which the Linux kernel uses to inspect, filter, forward, and translate network traffic.

---

# Part 3 - Stateful Firewall Verification

From PC1:

```text
ping 8.8.8.8
```

<img width="445" height="108" alt="image" src="https://github.com/user-attachments/assets/d10b425f-38e8-4b1c-bbd9-72deb46e24f3" />

### Questions

**Why does the firewall allow reply traffic without creating an explicit inbound rule?**

When a device sends out a packet, the firewall logs this outgoing packet in a state table. An incoming packet that comes from an IP address that is in this table bypasses the normal firewall block rules.

---

**What is the difference between a stateless firewall and a stateful firewall?**

A stateful firewall tracks active connection states and makes context-aware decisions, allowing return traffic for internal requests automatically. A stateless firewall evaluates each packet in absolute isolation based on static rules (like source/destination IP and ports) without remembering past packets

---

# Part 4 - WAN Protection

Determine the WAN interface status.

```bash
ifstatus wan
```

> **Insert Screenshot:** WAN Interface Information

Verify:

- WAN IP Address
- Default Gateway
- DNS Server

### Question

The WAN zone has an input policy of **REJECT**.

**What does this mean from a security perspective?**

> **Your Answer:**

---

# Part 5 - NAT (Masquerading)

Verify the firewall is performing source NAT.

```bash
nft list ruleset | grep masquerade
```

> **Insert Screenshot:** Masquerade Rule

### Questions

**Why is masquerading required for Internet access?**

> **Your Answer:**

---

**What would happen if masquerading were disabled?**

> **Your Answer:**

---

# Part 6 - Service Verification

Verify the required services.

```bash
service firewall enabled
service frr enabled
```

> **Insert Screenshot:** Services Enabled

---

Verify OSPF neighbors.

```text
show ip ospf neighbor
```

> **Insert Screenshot:** OSPF Neighbors

### Question

**Why is it important that OSPF neighbors remain in the FULL state after firewall configuration?**

> **Your Answer:**

---

# Part 7 - Connectivity Verification

## Connectivity Tests

| Test | Result |
|------|:------:|
| PC1 → PC2 | ✅ / ❌ |
| PC1 → PC3 | ✅ / ❌ |
| PC1 → Internet | ✅ / ❌ |
| PC2 → Internet | ✅ / ❌ |
| PC3 → Internet | ✅ / ❌ |

> **Insert Screenshot:** Successful Internet Connectivity

---

# Challenges Encountered

## Issue 1 - OSPF Neighbors Stuck in INIT

**Problem:**

> **Your Answer:**

**Resolution:**

> **Your Answer:**

---

## Issue 2 - Internet Connectivity Lost

**Problem:**

> **Your Answer:**

**Resolution:**

> **Your Answer:**

---

## Issue 3 - Transit Interfaces Not Assigned to Firewall Zones

**Problem:**

> **Your Answer:**

**Resolution:**

> **Your Answer:**

---

# Lessons Learned

### Lesson 1

> **Your Answer:**

---

### Lesson 2

> **Your Answer:**

---

### Lesson 3

> **Your Answer:**

---

### Lesson 4

> **Your Answer:**

---

# Key Takeaways

- OpenWrt organizes firewall policies using security zones rather than individual interfaces.
- Firewall4 converts UCI configuration into nftables rules.
- NAT (Masquerading) enables private IP networks to communicate with the Internet.
- Dynamic routing protocols such as OSPF require trusted communication across router transit links.
- Firewall configuration and routing configuration must be designed together to maintain both security and connectivity.

---

# Conclusion

This lab demonstrated how the OpenWrt firewall protects an enterprise network while allowing legitimate traffic to pass. Firewall zones, forwarding policies, NAT, and stateful inspection were examined using the three-router enterprise topology developed throughout the previous labs. Additionally, troubleshooting the interaction between firewall zones and OSPF highlighted the importance of assigning router transit interfaces to trusted firewall zones when deploying dynamic routing protocols.

---

# Suggested Screenshots

- [ ] Complete GNS3 Topology
- [ ] `uci show firewall.@zone[0]`
- [ ] `uci show firewall.@zone[1]`
- [ ] `fw4 print`
- [ ] `nft list ruleset`
- [ ] `nft list ruleset | grep masquerade`
- [ ] `ifstatus wan`
- [ ] `show ip ospf neighbor`
- [ ] Successful Internet Ping from a VPCS
