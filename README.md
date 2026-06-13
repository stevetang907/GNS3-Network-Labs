# Lab 01 - Basic Switching
## Objective

Create a simple switched network consisting of two hosts connected through an Ethernet switch and verify end-to-end connectivity.

## Topology

<img width="1493" height="703" alt="image" src="https://github.com/user-attachments/assets/5a0bc12e-c336-41fd-abd8-ea37df3295ea" />

## Devices Used
1 Ethernet Switch
2 VPCS Hosts
1 NAT Device

## IP Addressing

| Device | IP Address |
| --- | --- |
| PC1 | 192.168.2.128 |
| PC2 | 192.168.2.129 |
| Gateway | 192.168.2.1 |

## Configuration
### PC1

ip dhcp

### PC2

ip dhcp

## Verification
### PC1 to PC2 Ping

<img width="797" height="511" alt="image" src="https://github.com/user-attachments/assets/15414b42-3799-4a90-919c-bdbc3334ed1e" />

<img width="797" height="505" alt="image" src="https://github.com/user-attachments/assets/f3dd395a-fbdf-4fb4-9ccb-797d7655c1c2" />

### Internet Connectivity

Ping to 8.8.8.8 successful.

<img width="796" height="505" alt="image" src="https://github.com/user-attachments/assets/696550bb-6822-4cd8-90e1-04ddc7dd0f45" />

<img width="797" height="511" alt="image" src="https://github.com/user-attachments/assets/2d8ca38f-7f3c-4c36-b30d-019844eeabdd" />


## Lessons Learned
- Learned how DHCP assigns addresses.
- Learned how switches forward traffic.
- Learned how to verify connectivity using ping.
