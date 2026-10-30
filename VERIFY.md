# Repeatable lab verification

Status: procedure only. No new simulation results have been captured for this
checklist. Existing screenshots demonstrate ICMP behavior, not HTTP access.
Use only the supplied simulated Packet Tracer lab.

## 1. Verify the saved configuration

Open `Home_Network_Security_Lab.pkt`. On Router0, enter privileged EXEC mode
with `enable`, then run:

```text
show ip interface brief
show ip interface gigabitEthernet0/0
show access-lists 100
show running-config
```

Expected, not newly observed: G0/0 is 192.168.1.1 and G0/1 is 192.168.2.1,
both up/up. ACL 100 is inbound on G0/0, denies IP from 192.168.1.20 to
192.168.2.100, then permits other IP traffic. Compare with router_config.txt.
Check each endpoint's Desktop > IP Configuration: trusted 192.168.1.10/24,
guest 192.168.1.20/24, server 192.168.2.100/24. PC gateways are 192.168.1.1;
server gateway is 192.168.2.1.

If a link is down, inspect cabling, ports, and `no shutdown` on the router.
Allow link convergence before testing. Capture a fresh topology with working
links and readable host/IP labels, plus the interface-status output.

## 2. Repeat ICMP checks

Record `show access-lists 100` counters before testing. From each PC run:

```text
ping 192.168.2.100
```

Expected: trusted receives replies; guest receives no server replies. Repeat
trusted ping after initial address resolution if needed; retain both outputs.
Record ACL counters after each test and calculate their increase, rather than
assuming a particular total. Keep unexpected results and troubleshoot them.

## 3. Test HTTP separately

On Server0 select Services > HTTP and enable HTTP. From the trusted PC's
Desktop > Web Browser open `http://192.168.2.100` and confirm the server page
loads. This verifies the service is available before testing the deny rule.
Record ACL counters, then open the same URL from the guest PC. Expected:
the guest cannot load the page, and the matching deny counter increases.
Use Simulation mode, filter HTTP/TCP, and inspect the packet at Router0 to
show where it is dropped. Capture both browser outcomes and router evidence.
A browser timeout alone does not establish that the ACL caused the failure.
Retest trusted HTTP after the guest attempt as a control.

## 4. Save actual evidence

Save the updated .pkt file only after verification. Replace network_topology.png
with the fresh working-state screenshot. Add interface_status.txt,
http_trusted.png, http_guest.png, and acl_http_counters.txt (before/after counters
and test order). Record Packet Tracer version and test date.

Fill in this table with actual observations; do not copy the expected outcome
into the observed column without running the test.

| Test | Expected | Observed | Evidence |
|---|---|---|---|
| Router interfaces | G0/0 and G0/1 up/up | Not run | Pending |
| Trusted ping | Server replies | Not run in this repeat | Pending |
| Guest ping | No server replies; deny counter increases | Not run in this repeat | Pending |
| Trusted HTTP | Server page loads | Not run | Pending |
| Guest HTTP | Page blocked; router drops matching TCP traffic | Not run | Pending |

Update README testing claims only after saving evidence. HTTP tests establish
behavior for the tested TCP service, not UDP or all applications. This remains
a host-to-server ACL lab, not full guest isolation: both PCs share the same LAN.
