# Lab 02 - Static IP Addressing
## Objective

Configure static IP addresses on multiple hosts and verify end-to-end connectivity without DHCP.

## Skills Practiced
- IPv4 Addressing
- Subnet Masks
- Default Gateways
- Connectivity Testing

## Topology

<img width="1493" height="703" alt="image" src="https://github.com/user-attachments/assets/f3837ab6-641e-4ecd-b51d-d6f6845dfc31" />

## Devices Used
- 1 Ethernet Switch
- 2 VPCS Hosts
- 1 NAT Device
## Addressing Table
| Device | IP Address	| Subnet Mask	| Gateway |
| --- | --- | --- | --- |
| PC1	| 192.168.2.10 | 255.255.255.0 | 192.168.2.1 |
| PC2	| 192.168.2.20 | 255.255.255.0 | 192.168.2.1 |

## Configuration

### PC1

ip 192.168.2.10/24 192.168.2.1

### PC2

ip 192.168.2.20/24 192.168.2.1

## Verification
### Show IP Configuration

<img width="528" height="357" alt="image" src="https://github.com/user-attachments/assets/d1482d94-6d29-4f43-be17-d4802df989f8" />

<img width="587" height="545" alt="image" src="https://github.com/user-attachments/assets/c8e5b7d9-007f-46ca-ac4b-0a0d22f3aca1" />


## Ping Tests

<img width="596" height="136" alt="image" src="https://github.com/user-attachments/assets/eb24e04c-0722-4901-87f1-209cb6169109" />

<img width="608" height="127" alt="image" src="https://github.com/user-attachments/assets/562c3f7d-3c35-457b-9b8a-d0769e3a2f8d" />


## Troubleshooting Notes

No issues encountered.

## Lessons Learned
- Difference between static and dynamic addressing
- Importance of default gateways
- Basic connectivity testing
