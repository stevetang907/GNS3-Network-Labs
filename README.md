# Lab 03 - Linux Host Networking
## Objective

Deploy a Linux host in GNS3 and verify network connectivity to local and external destinations.

## Skills Practiced
- Linux Networking
- IP Configuration
- Ping Testing
- Traceroute
- Basic Linux Administration

## Topology
<img width="1498" height="706" alt="Screenshot 2026-06-15 203337" src="https://github.com/user-attachments/assets/681d75f5-e9e3-457c-8423-109f68eb6662" />

## Devices Used
- Linux Host
- Ethernet Switch
- NAT Device
  
## Addressing Information
| Device | Address|
| --- | --- |
| Linux Host | DHCP Assigned |
| Gateway |	DHCP Assigned |
## Commands Used
### View IP Configuration

ifconfig

### Verify Connectivity

ping 8.8.8.8

### Trace Route

traceroute 8.8.8.8

## Verification

<img width="637" height="420" alt="image" src="https://github.com/user-attachments/assets/b7c3436b-8b78-4c58-9f6a-09304f935b58" />
<img width="621" height="251" alt="image" src="https://github.com/user-attachments/assets/295760de-1375-48ab-86c4-ce9c0e33aa07" />


## Troubleshooting Notes

Had trouble getting Linux machine to start because of KVM error. I initially tried disabling KVM in just GNS3, but it didn't work. I had to go into the server configuration for the GNS3 VM to add the following to the server configuration:  
<img width="450" height="101" alt="image" src="https://github.com/user-attachments/assets/d7608f97-0a59-4d8e-814c-1420926ba94d" />


## Lessons Learned
- Linux network commands
- Network troubleshooting basics
- Route tracing fundamentals
