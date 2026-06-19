# Lab 05 - Multi-Network Routing
## Objective

Configure routing between multiple subnets using a router.

## Skills Practiced
- Inter-Network Routing
- Multiple Interfaces
- IP Planning
- Connectivity Verification

## Topology

<img width="1842" height="731" alt="image" src="https://github.com/user-attachments/assets/c236b748-a7e2-406b-8d2a-507ad9b4e2ce" />

## Devices Used
- Router
- PC1
- PC2

## Addressing Table
| Device | Interface | Network |
| --- | --- | --- |
| PC1	| NIC | 192.168.10.0/24 |
| OpenWRT | eth0 | 192.168.10.1 |
| PC2	| NIC | 192.168.20.0/24 |
| OpenWRT | eth 3 | 192.162.20.1 |

## Configuration Summary
### Router

<img width="1493" height="700" alt="Screenshot 2026-06-19 174251" src="https://github.com/user-attachments/assets/9a318486-e443-48ac-8efc-c4ca491eac74" />

### Hosts

PC1: ip 192.168.10.10/24 192.168.10.1
PC2: ip 192.168.20.10/24 192.168.20.1

## Verification
### Routing Table

<img width="560" height="101" alt="image" src="https://github.com/user-attachments/assets/cbd9d0cd-d0c0-4864-8d33-37b2764df6b9" />


## Ping Tests
PC1 → PC2  
<img width="605" height="136" alt="image" src="https://github.com/user-attachments/assets/1bd986c3-5151-4b0a-a61f-6ac4b6a56365" />

PC2 → PC1  
<img width="606" height="122" alt="image" src="https://github.com/user-attachments/assets/771946c5-8d15-4225-ae9e-2ce57db6e34c" />

## Troubleshooting Notes

Initial connectivity failed between subnets due to OpenWRT firewall zone isolation. Although routing tables were correct, inter-zone forwarding was disabled. After enabling forwarding between LAN interfaces, connectivity was restored. Fixed by running the following in OpenWRT console:  
uci set firewall.@defaults[0].forward='ACCEPT'  
uci commit firewall  
/etc/init.d/firewall restart  

## Lessons Learned
- Subnet separation
- Layer 3 forwarding
- Router interface design
