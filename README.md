# Lab 04 - Introduction to Routing
## Objective

Configure a router between a client network and an upstream network and verify routing functionality.

## Skills Practiced
- Router Interface Configuration
- Static Routing
- Default Routes
- Route Verification

## Topology

<img width="1852" height="728" alt="image" src="https://github.com/user-attachments/assets/d685e781-94e1-423a-91a4-76957c9e82ad" />


## Devices Used
- Router (OpenWrt)
- VPCS Host
- NAT Device
## Addressing Table
| Interface	| Address |
| --- | --- |
| Router LAN | 192.168.1.1/24 |
| Router WAN | 192.168.2.132/24 |
| PC1	| 192.168.10.10/24 |
## Configuration Summary
### Router

- Use cat /etc/config/network to find LAN and WAN interfaces that correspond to each Ethernet port
- Use ip route to show the routing table
PC1

ip 192.168.10.10/24 192.168.1.1

## Verification
### Routing Table

<img width="537" height="78" alt="Screenshot 2026-06-16 201447" src="https://github.com/user-attachments/assets/9cbf7c91-7475-48b8-9080-1e3b931ad168" />
<img width="1006" height="698" alt="image" src="https://github.com/user-attachments/assets/2acc8107-b7f5-4848-adc9-abf1a530d85d" />

### Connectivity Test

<img width="592" height="276" alt="image" src="https://github.com/user-attachments/assets/9839a925-1324-4dd4-8008-0f970f959692" />
<img width="606" height="268" alt="image" src="https://github.com/user-attachments/assets/de1dff29-f80a-4a13-8d6f-69854c178b7c" />

## Troubleshooting Notes
- Had trouble figuring out which ethernet ports were LAN or WAN interfaces on the router. Used cat /etc/config/network to figure it out.
- Didn't realize that the WAN interface needed its own connection the switch. Connected both the LAN and WAN interfaces to the switch on eth0 and eth1, respectively.

## Lessons Learned
- How routers forward traffic
- Importance of routing tables
- Difference between local and remote networks
