# DoS_Roteador-HG8145V5-V2

Presentation:

Security vulnerability: WAN Connectivity Denial of Service
Vulnerability Type: Resource Exhaustion / Network State Handling Failure
CWE: CWE-400 (Uncontrolled Resource Consumption) / CWE-404 (Improper Resource Shutdown or Release)
CVE: Pending Assignment
Affected Component: WAN forwarding plane / NAT session handling
Software: Huawei EchoLife HG8145V5 GPON Terminal
Version: V5R022C10S293 (Hardware 26AD.A – OI2WIFI Custom)
Business area: Telecommunications / ISP Infrastructure / FTTH
Submitter: bl4dsc4n

Overview

A denial-of-service condition was identified in the Huawei EchoLife HG8145V5 GPON terminal running firmware V5R022C10S293.

Under sustained high-rate outbound traffic (e.g., scanning or fuzzing activity) generated from a LAN-connected host, the device experiences complete WAN connectivity loss affecting all connected clients.

During the stress condition:

The device remains reachable via its local management interface (192.168.100.1)

ICMP to the local gateway continues to function

ICMP to external hosts (e.g., 8.8.8.8) fails

All outbound TCP sessions fail with connection timeouts

Multiple LAN devices (PC and mobile) simultaneously lose Internet access

Connectivity is immediately restored once the high-rate traffic is stopped.

This behavior indicates degradation or exhaustion within the WAN forwarding path or internal session/state handling mechanisms rather than a full system crash.

Impact

Complete loss of Internet connectivity

All LAN-connected devices affected

No authentication required (LAN-adjacent trigger)

High availability impact

The device does not reboot or crash, but effectively becomes unable to route external traffic while under stress.

Technical Details

Observed Environment:

Device: Huawei EchoLife HG8145V5

Firmware: V5R022C10S293

Hardware: 26AD.A

Custom Info: OI2WIFI

ONT Status: O5 (Operational)

CPU Usage during test: ~4%

Memory Usage: ~63%

Behavior Characteristics:

LAN control plane remains responsive

WAN data plane becomes unavailable

Recovery is immediate upon traffic cessation

No reboot required

This suggests potential:

NAT/Conntrack table exhaustion

Session state mismanagement

Forwarding plane degradation under stress

Proof of Concept

The issue can be reproduced by generating sustained high-rate outbound traffic from a single LAN host targeting external Internet services.

PoC demonstration shows:

Simultaneous WAN loss for multiple devices

Local management interface remains accessible

External ICMP and TCP fail

Immediate recovery when stress traffic stops

PoC video available for responsible disclosure validation.

Mitigation

Until an official firmware fix is released:

Disable UPnP if not required

Apply outbound connection rate limiting

Monitor session counts and abnormal traffic

Apply firmware updates when available

Restrict high-rate traffic generation from LAN clients
