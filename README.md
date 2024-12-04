# Multiarea OSPF Configuration with IPv4 & IPv6

## Contents

1. [**Purpose**](#purpose)
2. [**Background**](#background)
3. [**Topology**](#topology)
     1. [**IPv4 Topology**](#ipv4-topology)
     2. [**IPv6 Topology**](#ipv6-topology)
4. [**Address Table**](#address-table)
5. [**Device Overview**](#device-overview)
6. [**ICMPv4 Traceroute Across Network**](#icmpv4-traceroute-across-network-from-pc0)
7. [**ICMPv6 Traceroute Across Network**](#icmpv6-traceroute-across-network-from-pc0)
8. [**IPv4 Routing Table**](#ipv4-topology)
9. [**IPv6 Routing Table**](#ipv6-topology)

## Purpose

The purpose of this lab is to setup 3 AS’s and use eBGP to route between the 3 AS’s. Students will learn how to redistribute routes between gateway protocols and routing protocols, what address-families are, and how to declare eBGP neighbors. Students will also brush up on skills needed to setup networks with routing protocols, including advertising interfaces, subnetting, and debugging. Additionally, Students may learn how to configure routing protocols they have not used before for their separate AS’s.

## Background

This is section is background info on key concept/parts of the configuration. It is directed an audience that knows some networking, but their knowledge is limited.

### Routing Protocols

Routing protocols can automate the process of getting destination routes to other networks in some manner, requiring some initial setup.

### eBGP

Exterior Border Gateway Protocol (eBGP) is an exterior gateway protocol, which means that it can share routing information between autonomous systems (AS’s).

- Each AS has one process/AS of a routing protocol running for it. For example, one organization uses OSPF, while another uses OSPF in a different process or a different routing protocol; these two organizations can establish communication between their own AS’s by using eBGP.

What allows eBGP to communicate between AS’s is its ability to redistribute routes. Routes can be redistributed into eBGP, then eBGP can redistribute those routes into any other routing protocol. This is extremely important in that Internet Service Providers (ISPs) may use different routing protocols, so eBGP can help them share routing information. But another problem can crop up: having multiple routes to one destination. Although this isn’t a problem in and of itself, it’s important for eBGP to choose a route logically. There are many ways to manually influence the path selection for eBGP, like weight and local preference. If eBGP is not manually set to influence the path selection, it will depend on a series of attributes to determine the shortest path to the destination. (these attributes are not listed, since there are eleven of them)

### OSPF

Open Shortest Path First (OSPF) is a link-state routing protocol for Internet Protocol (IP). OSPF supplies destination routes to its routers by gathering information from the links on each router; then each router will forward that link-state information throughout the entire network. Other routers will use this information to create a topology of the network, which will supply the routes for each router.

### EIGRP

Enhanced Interior Gateway Routing Protocol (EIGRP) is a distance-vector routing protocol that uses distance, calculated with bandwidth and delay, for routing decisions in the network. EIGRP keeps a topology table created with information gathered by neighbors, which EIGRP will use to decide which routes to give to the router. It is worth noting that unlike other distance-vector routing protocols, EIGRP doesn’t send its table periodically, but only when a topology or link-state change has occurred.

### IS-IS

Intermediate System to Intermediate System (IS-IS) is also a link-state routing protocol. Though there are some differences between it and OSPF: IS-IS can route for IPv4 and IPv6 in the same instance and IS-IS can do partial route calculation, allowing for the network to converge quicker. Other than these differences, IS-IS and OSPF are quite similar protocols.

## Topology

These are the topologies for both IPv4 and IPv6, each link is labeled with the network number and subnet mask of the link. Then each interface is labeled with the last octet or hextet of the usable IPv4/IPv6 address within the subnet of that link.\
Additionally, the PCs can have any IP that is within the subnet of the link that they are on. **DHCP is not setup**.

### <center>IPv4 Topology</center>

![IPv4 Topology Image](Images/IPv4.Topology.png)

### <center>IPv6 Topology</center>

![IPv4 Topology Image](Images/IPv6.Topology.png)

## Address Table

| Device Name | Interface | IPv6 Address | IPv4 Address | Subnet Mask   |
|:------------|:--------- |:------------ |:------------ |:------------- |
| R1          | G0/0/0    | 1:20::1/64   | 10.0.20.1    | 255.255.255.0 |
| R1          | G0/0/1    | 1::1/64      | 10.0.0.1     | 255.255.255.0 |
| R2          | G0/0/0    | 1::2/64      | 10.0.0.2     | 255.255.255.0 |
| R2          | G0/0/1    | 2::1/64      | 192.168.0.1  | 255.255.255.0 |
| R3          | G0/0/0    | 2::2/64      | 192.168.0.2  | 255.255.255.0 |
| R3          | G0/0/1    | 1:1::1/64    | 10.0.1.1     | 255.255.255.0 |
| R4          | G0/0/0    | 1:1::2/64    | 10.0.1.2     | 255.255.255.0 |
| R4          | G0/0/1    | 2:1::1/64    | 192.168.1.1  | 255.255.255.0 |
| R5          | G0/0/0    | 2:1::2/64    | 192.168.1.2  | 255.255.255.0 |
| R5          | G0/0/1    | 1:3::1/64    | 10.0.3.1     | 255.255.255.0 |
| R6          | G0/0/0    | 1:3::2/64    | 10.0.3.2     | 255.255.255.0 |
| R6          | G0/0/1    | 1:30::1/64   | 10.0.30.1    | 255.255.255.0 |

## Device Overview

This Topology Consists of...

- Six 4321 routers running Cisco IOS XE Software, Version 16.9 Universal K9

## ICMPv4 Traceroute Across Network From PC0

```text
C:\>tracert 10.0.30.2

Tracing route to DESKTOP-43DJSK3 [10.0.30.2]
over a maximum of 30 hops:

  1    3 ms   <1 ms    <1 ms  10.0.20.1
  2   <1 ms   <1 ms    <1 ms  10.0.0.2
  3   <1 ms    1 ms    <1 ms  192.168.0.2
  4   <1 ms   <1 ms    <1 ms  10.0.1.2
  5   <1 ms   <1 ms    <1 ms  192.168.1.2
  6    1 ms    1 ms     1 ms  10.0.3.2
  7    1 ms   <1 ms     1 ms  DESKTOP-43DJSK3 [10.0.30.2]

Trace complete.
```

## ICMPv6 Traceroute Across Network From PC0

```text
C:\>tracert 1:30::2

Tracing route to 1:30::2 over a maximum of 30 hops

  1   <1 ms   <1 ms    <1 ms  1:20::1
  2    1 ms   <1 ms    <1 ms  1::1
  3    1 ms   <1 ms    <1 ms  2::2
  4    1 ms    1 ms    <1 ms  1:1::2
  5    1 ms   <1 ms     1 ms  2:1::2
  6    1 ms    1 ms     1 ms  1:3::2
  7    1 ms    1 ms     1 ms  1:30::2

Trace complete.
```

## R1 IPv4 Routing Table

```text
R1#show ip route
Codes: L - local, C - connected, S - static, R - RIP, M - mobile, B - BGP
       D - EIGRP, EX - EIGRP external, O - OSPF, IA - OSPF inter area
       N1 - OSPF NSSA external type 1, N2 - OSPF NSSA external type 2
       E1 - OSPF external type 1, E2 - OSPF external type 2
       i - IS-IS, su - IS-IS summary, L1 - IS-IS level-1, L2 - IS-IS level-2
       ia - IS-IS inter area, * - candidate default, U - per-user static route
       o - ODR, P - periodic downloaded static route, H - NHRP, l - LISP
       a - application route
       + - replicated route, % - next hop override, p - overrides from PfR

Gateway of last resort is not set

      10.0.0.0/8 is variably subnetted, 7 subnets, 2 masks
C        10.0.0.0/24 is directly connected, GigabitEthernet0/0/1
L        10.0.0.1/32 is directly connected, GigabitEthernet0/0/1
O E2     10.0.1.0/24 [110/10] via 10.0.0.2, 01:07:00, GigabitEthernet0/0/1
O E2     10.0.3.0/24 [110/10] via 10.0.0.2, 01:06:30, GigabitEthernet0/0/1
C        10.0.20.0/24 is directly connected, GigabitEthernet0/0/0
L        10.0.20.1/32 is directly connected, GigabitEthernet0/0/0
O E2     10.0.30.0/24 [110/10] via 10.0.0.2, 01:06:30, GigabitEthernet0/0/1
O     192.168.0.0/24 [110/2] via 10.0.0.2, 01:09:00, GigabitEthernet0/0/1
O E2  192.168.1.0/24 [110/10] via 10.0.0.2, 01:06:30, GigabitEthernet0/0/1
```

## R1 IPv6 Routing Table

```text
R1#show ipv6 route
IPv6 Routing Table - default - 9 entries
Codes: C - Connected, L - Local, S - Static, U - Per-user Static route
       B - BGP, R - RIP, I1 - ISIS L1, I2 - ISIS L2
       IA - ISIS interarea, IS - ISIS summary, D - EIGRP, EX - EIGRP external
       ND - ND Default, NDp - ND Prefix, DCE - Destination, NDr - Redirect
       O - OSPF Intra, OI - OSPF Inter, OE1 - OSPF ext 1, OE2 - OSPF ext 2
       ON1 - OSPF NSSA ext 1, ON2 - OSPF NSSA ext 2, a - Application
C   1::/64 [0/0]
     via GigabitEthernet0/0/1, directly connected
OE2 1:1::/64 [110/10]
     via FE80::CE7F:76FF:FE6A:B5E0, GigabitEthernet0/0/1
OE2 1:3::/64 [110/10]
     via FE80::CE7F:76FF:FE6A:B5E0, GigabitEthernet0/0/1
C   1:20::/64 [0/0]
     via GigabitEthernet0/0/0, directly connected
L   1:20::1/128 [0/0]
     via GigabitEthernet0/0/0, receive
OE2 1:30::/64 [110/10]
     via FE80::CE7F:76FF:FE6A:B5E0, GigabitEthernet0/0/1
O   2::/64 [110/2]
     via FE80::CE7F:76FF:FE6A:B5E0, GigabitEthernet0/0/1
OE2 2:1::/64 [110/10]
     via FE80::CE7F:76FF:FE6A:B5E0, GigabitEthernet0/0/1
L   FF00::/8 [0/0]
     via Null0, receive
```
