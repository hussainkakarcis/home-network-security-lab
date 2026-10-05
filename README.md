# Home Network Security Lab

## Overview
This project is a small network security lab built in Cisco Packet Tracer. I created two separate IPv4 subnets connected through a router, verified communication between devices, and then applied an extended access control list (ACL) to block a guest PC from accessing a protected server while still allowing trusted traffic.

## Network Design
- Trusted PC: `192.168.1.10/24`
- Guest PC: `192.168.1.20/24`
- Router G0/0: `192.168.1.1/24`
- Router G0/1: `192.168.2.1/24`
- Server: `192.168.2.100/24`

## Security Rule
The extended ACL blocks traffic from the guest PC to the protected server and permits other IP traffic:

```text
access-list 100 deny ip host 192.168.1.20 host 192.168.2.100
access-list 100 permit ip any any
interface gigabitEthernet0/0
ip access-group 100 in
```

## Testing
Before applying the ACL, both PCs could reach the server.

After applying the ACL:
- Trusted PC -> Server: allowed
- Guest PC -> Server: blocked

I verified the behavior with ping tests and the router command:

```text
show access-lists
```

The ACL match counters confirmed that the deny rule was actively filtering traffic.

## Files
- `Home_Network_Security_Lab.pkt` - Cisco Packet Tracer project
- `screenshots/network_topology.png` - full lab topology
- `screenshots/trusted_pc_ping_success.png` - trusted PC successfully reaching the server
- `screenshots/guest_pc_ping_blocked.png` - guest PC blocked from the server
- `screenshots/acl_verification.png` - ACL configuration and match counters

## Skills Demonstrated
- Cisco Packet Tracer
- IPv4 addressing and subnetting
- Router interface configuration
- Routing between subnets
- Extended access control lists
- Traffic filtering
- Network troubleshooting and verification

## Screenshots

### Network Topology
![Network Topology](screenshots/network_topology.png)

### Trusted PC - Successful Ping
![Trusted PC Ping](screenshots/trusted_pc_ping_success.png)

### Guest PC - Blocked Ping
![Guest PC Blocked](screenshots/guest_pc_ping_blocked.png)

### ACL Verification
![ACL Verification](screenshots/acl_verification.png)
