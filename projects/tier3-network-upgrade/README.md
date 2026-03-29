# Tier 3 Network Upgrade

## Overview
This project documents the transition from a flat network architecture to a structured Tier 3 design to improve resiliency, scalability, and manageability.

---

## Objectives
- Eliminate flat network bottlenecks
- Introduce core/distribution/access segmentation
- Improve redundancy and failover capability
- Prepare infrastructure for future growth (10/25Gb backbone)

---

## Architecture Summary

- Dual core aggregation switches
- Redundant firewall uplinks (HA pair)
- Segmented VLAN design
- High-speed backbone (10Gb / 25Gb)

---

## High-Level Topology

```mermaid
graph TD

    ISP1["ISP - UP.net"]
    ISP2["ISP - Spectrum"]

    FW1["Firewall HA - Primary"]
    FW2["Firewall HA - Secondary"]

    CORE1["Core Switch 1"]
    CORE2["Core Switch 2"]

    ACCESS1["Access Switch Stack A"]
    ACCESS2["Access Switch Stack B"]

    SERVERS["VMware Hosts / SAN"]
    WLAN["Wireless Infrastructure"]

    ISP1 --> FW1
    ISP2 --> FW1

    FW1 --> CORE1
    FW1 --> CORE2

    FW2 --> CORE1
    FW2 --> CORE2

    CORE1 --> CORE2

    CORE1 --> ACCESS1
    CORE2 --> ACCESS2

    CORE1 --> SERVERS
    CORE2 --> SERVERS

    CORE1 --> WLAN
    CORE2 --> WLAN
