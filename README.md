# Lab 07 - Static Routing Between Multiple Routers
## Objective

Configure static routing between two OpenWrt routers to enable communication between two separate LAN networks connected through a point-to-point transit network.

## Skills Practiced
- Static Routing
- Inter-Router Communication
- Point-to-Point Network Design
- IP Address Planning
- Routing Table Verification
- Network Troubleshooting
- OpenWrt Interface Configuration
- Firewall Troubleshooting

## Topology

<img width="1277" height="710" alt="image" src="https://github.com/user-attachments/assets/7da6c78e-4968-4b1b-865c-b340f986570a" />

## Devices Used
- OpenWRT Router 1
- OpenWRT Router 2
- PC1
- PC2
- GNS3 Switch 1
- GNS3 Switch 2

## Addressing Table
| Device | Interface | IP Address | Network |
| --- | --- | --- | --- |
| PC1	| NIC | 192.168.10.10/24 | 192.168.10.0/24 |
| Router 1 | br-lan(eth0) | 192.168.10.1/24 | 192.168.10.0/24 |
| Router 1 | eth1 | 10.0.12.1/30 | 10.0.12.0/30 |
| Router 2 | br-lan(eth0) | 192.168.20.1/24 | 192.168.20.0/24 |
| Router 2 | eth1 | 10.0.12.2/30 | 10.0.12.0/30 |
| PC2	| NIC | 192.168.20.10/24 | 192.168.20.0/24 |

## Configuration Summary
### Router 1

<img width="876" height="558" alt="image" src="https://github.com/user-attachments/assets/7f4a5969-b172-470b-8120-f55c7e620d42" />

### Router 2
<img width="810" height="556" alt="image" src="https://github.com/user-attachments/assets/f72625b2-3fff-4ce8-989a-526a3727e6a3" />

### Hosts

PC1:  
IP Address: 192.168.10.10/24   
Gateway: 192.168.10.1  

PC2:  
IP Address: 192.168.20.10/24  
Gateway: 192.168.20.1

## Verification
### Routing Table

Router 1:  
<img width="557" height="80" alt="Screenshot 2026-07-21 205133" src="https://github.com/user-attachments/assets/d5feef61-cc16-49ef-9e1f-a844d50435d5" />

Router 2:  
<img width="557" height="80" alt="image" src="https://github.com/user-attachments/assets/0aa8c8ce-d6f9-4cb7-bd2b-b728543c0cd2" />



## Ping Tests
Router 1 → Router 2 Transit Link:  
<img width="613" height="240" alt="Screenshot 2026-07-21 205421" src="https://github.com/user-attachments/assets/06e1bc25-060a-494b-97f8-c8420d037327" />

Router 2 → Router 1 Transit Link:  
<img width="578" height="215" alt="image" src="https://github.com/user-attachments/assets/13c9dbcc-397d-4665-9efa-41a36b5a50a3" />

PC1 → Router 2 LAN Interface:  
<img width="597" height="122" alt="image" src="https://github.com/user-attachments/assets/c6193e44-3ebf-4ffa-8ed1-a69adecc7fd1" />

PC2 → Router 1 LAN Interface:  
<img width="580" height="122" alt="image" src="https://github.com/user-attachments/assets/4b572ff9-29a1-4899-a782-f8e82c7c5285" />

PC1 → PC2:  
<img width="606" height="125" alt="image" src="https://github.com/user-attachments/assets/2197fab9-5a09-4770-92b8-5ce662ab4355" />

PC2 → PC1:  
<img width="602" height="122" alt="image" src="https://github.com/user-attachments/assets/2652bc96-fe0b-43bf-b053-8bb325058245" />

## Troubleshooting Notes

Initial connectivity testing failed even though interface configuration and routing were correct.

Troubleshooting steps performed:

- Verified router interfaces using: ip addr
- Verified routing tables using: ip route
- Confirmed Layer 2 connectivity using ARP: arp -n

The routers successfully learned each other's MAC addresses, confirming that the transit link was functioning.

The issue was caused by the default OpenWrt firewall configuration. The firewall treated the transit interface (eth1) as a WAN interface and blocked routed traffic between networks.

The issue was resolved by disabling the OpenWrt firewall:  

/etc/init.d/firewall stop  
/etc/init.d/firewall disable

After disabling the firewall, all router and host connectivity tests passed.

## Lessons Learned
- Static routes allow communication between separate networks without a dynamic routing protocol.
- Point-to-point links commonly use small subnets such as /30.
- Successful ARP resolution confirms Layer 2 connectivity but does not guarantee Layer 3 communication.
- Routing tables must contain a path to remote networks.
- Firewalls can block traffic even when routing is correctly configured.
- Troubleshooting should follow a layered approach:
  - Layer 1: Link status
  - Layer 2: ARP/MAC learning
  - Layer 3: IP addressing and routing
  - Layer 4+: Firewall and service behavior
