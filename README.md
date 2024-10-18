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

## Background

This is section is background info on key concept/parts of the configuration. It is directed an audience that knows some networking, but their knowledge is limited.

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

## R1 IPv6 Routing Table
