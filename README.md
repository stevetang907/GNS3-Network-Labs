# Lab 08 - Three-Router Enterprise Network
## Overview
This lab expands on the previous static routing lab by introducing a three-router enterprise topology. Three LANs are connected using two point-to-point transit networks, allowing end-to-end communication between all hosts through static routing.

The objective was to understand how routing tables scale as networks grow and how routers forward traffic across multiple hops.

## Objectives
- Build a three-router enterprise topology in GNS3
- Configure three independent LANs
- Configure two point-to-point transit networks
- Configure static routes on each router
- Verify end-to-end connectivity
- Practice troubleshooting routing and firewall issues

## Skills Practiced
- Static routing
- Multi-hop routing
- Route table analysis
- Enterprise topology design
- IP addressing
- Transit networks (/30)
- GNS3 troubleshooting
- OpenWrt configuration
- ICMP verification
- Network troubleshooting

## Topology

<img width="1280" height="792" alt="image" src="https://github.com/user-attachments/assets/ba3a5f68-66e5-4778-b1ff-26b4d7c9edff" />

## Devices Used
- OpenWRT Router 1
- OpenWRT Router 2
- OpenWRT Router 3
- PC1
- PC2
- PC3
- GNS3 Switch 1
- GNS3 Switch 2
- GNS3 Switch 3

## Addressing Table
| Device | Interface | IP Address | Network |
| --- | --- | --- | --- |
| PC1	| NIC | 192.168.10.10/24 | 192.168.10.0/24 |
| Router 1 | br-lan(eth0) | 192.168.10.1/24 | 192.168.10.0/24 |
| Router 1 | eth1 | 10.0.12.1/30 | 10.0.12.0/30 |
| Router 2 | br-lan(eth0) | 192.168.20.1/24 | 192.168.20.0/24 |
| Router 2 | eth1 | 10.0.12.2/30 | 10.0.12.0/30 |
| Router 2 | eth2 | 10.0.23.1/30 | 10.0.23.0/30 |
| PC2	| NIC | 192.168.20.10/24 | 192.168.20.0/24 |
| Router 3 | br-lan(eth0) | 192.168.30.1/24 | 192.168.30.0/24 |
| Router 3 | eth1 | 10.0.23.2/30 | 10.0.23.0/30 |
| PC2	| NIC | 192.168.30.10/24 | 192.168.30.0/24 |

## Static Routes
### Router 1
| Destination | Next Hop |
| --- | --- |
| 192.168.20.0/24 |	10.0.12.2 |
| 192.168.30.0/24 |	10.0.12.2 |
| 10.0.23.0/30 | 10.0.12.2 |

### Router 2
| Destination | Next Hop |
| --- | --- |
| 192.168.10.0/24 |	10.0.12.1 |
| 192.168.30.0/24 |	10.0.23.2 |

### Router 3
| Destination | Next Hop |
| --- | --- |
| 192.168.20.0/24 |	10.0.23.1 |
| 192.168.10.0/24 |	10.0.23.1 |
| 10.0.12.0/30 | 10.0.23.1 |


## Configuration Summary
### Router 1
<img width="876" height="558" alt="image" src="https://github.com/user-attachments/assets/7f4a5969-b172-470b-8120-f55c7e620d42" />

### Router 2
<img width="848" height="658" alt="image" src="https://github.com/user-attachments/assets/75c027df-c871-45d0-8b08-24b74b19a58d" />

### Router 3
<img width="822" height="552" alt="image" src="https://github.com/user-attachments/assets/069eefa9-79a7-4b3e-93e2-bb505e7510f1" />


### Hosts

PC1:  
IP Address: 192.168.10.10/24   
Gateway: 192.168.10.1  

PC2:  
IP Address: 192.168.20.10/24  
Gateway: 192.168.20.1

PC3:  
IP Address: 192.168.30.10/24  
Gateway: 192.168.30.1

## Verification
### Routing Table

Router 1:  
<img width="555" height="116" alt="image" src="https://github.com/user-attachments/assets/78873715-2329-4327-be19-758706b497f7" />

Router 2:  
<img width="551" height="115" alt="image" src="https://github.com/user-attachments/assets/d1866e5e-81eb-4cdb-b974-1ff3de2d0b65" />

Router 3:  
<img width="557" height="123" alt="image" src="https://github.com/user-attachments/assets/e6e83058-09be-49d4-8808-e480f4a60cec" />

## Ping Tests
Router 1 → Router 2 Transit Link:  
<img width="613" height="240" alt="Screenshot 2026-07-21 205421" src="https://github.com/user-attachments/assets/06e1bc25-060a-494b-97f8-c8420d037327" />

Router 1 → Router 3 Transit Link:  
<img width="515" height="161" alt="image" src="https://github.com/user-attachments/assets/32fd43c9-228d-411e-8ff4-663bab9aa773" />

Router 2 → Router 1 Transit Link:  
<img width="578" height="215" alt="image" src="https://github.com/user-attachments/assets/13c9dbcc-397d-4665-9efa-41a36b5a50a3" />

Router 2 → Router 3 Transit Link:  
<img width="578" height="221" alt="image" src="https://github.com/user-attachments/assets/4e867016-21c5-43b9-bec0-f24c3fd0b41f" />

Router 3 → Router 1 Transit Link:  
<img width="576" height="237" alt="image" src="https://github.com/user-attachments/assets/97986cb3-6968-4022-addb-f629162bc4b7" />

PC1 → Router 2 LAN Interface:  
<img width="597" height="122" alt="image" src="https://github.com/user-attachments/assets/c6193e44-3ebf-4ffa-8ed1-a69adecc7fd1" />

PC2 → Router 1 LAN Interface:  
<img width="580" height="122" alt="image" src="https://github.com/user-attachments/assets/4b572ff9-29a1-4899-a782-f8e82c7c5285" />

PC1 → PC2:  
<img width="606" height="125" alt="image" src="https://github.com/user-attachments/assets/2197fab9-5a09-4770-92b8-5ce662ab4355" />

PC2 → PC1:  
<img width="602" height="122" alt="image" src="https://github.com/user-attachments/assets/2652bc96-fe0b-43bf-b053-8bb325058245" />

## Troubleshooting Notes

Only directly connected networks appeared in each routing table, preventing communication with remote LANs.  

### Resolution  

Configured static routes on each router for all remote networks.

## Key Takeaways
This lab demonstrated how static routing becomes increasingly complex as additional routers and networks are introduced. While static routes are suitable for small environments, larger enterprise networks typically rely on dynamic routing protocols such as RIP or OSPF to automatically exchange routing information.

