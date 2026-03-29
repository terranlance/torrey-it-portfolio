## Key Design Decisions

### Dual Core Architecture
Two core switches were implemented to eliminate single points of failure and enable load distribution across the network.

### Firewall High Availability
A high-availability firewall pair was deployed with redundant uplinks to both core switches to ensure continuity during failover events.

### VLAN Segmentation
Traffic was segmented into logical networks to improve security and manageability:
- Management
- Servers
- Workstations
- Wireless
- Guest

### High-Speed Backbone
10Gb and 25Gb links were introduced to support:
- East-west traffic
- Storage workloads
- Future infrastructure scaling

---

## Challenges & Considerations

- Migrating from a flat network without service disruption
- Maintaining VLAN consistency across switching infrastructure
- Preventing asymmetric routing conditions
- Supporting legacy devices with mixed VLAN dependencies

---

## Lessons Learned

- Proper labeling and documentation significantly reduce migration risk
- Failover scenarios should be tested early in the process
- Simplicity in VLAN design improves long-term maintainability
- Routing and DHCP paths must be validated at each phase of deployment

---

## Future Improvements

- Implement a dedicated management VLAN with restricted access
- Deploy 802.1X for network access control
- Further segment medical and IoT devices
- Enhance monitoring and telemetry visibility

- ## High-Level Topology

```mermaid
graph TD

    %% ISPs
    ISP1["ISP - UP.net"]
    ISP2["ISP - Spectrum"]

    %% Firewalls (HA Pair)
    FW1["Firewall HA - Primary"]
    FW2["Firewall HA - Secondary"]

    %% Core Layer
    CORE1["Core Switch 1"]
    CORE2["Core Switch 2"]

    %% Access Layer
    ACCESS1["Access Stack A"]
    ACCESS2["Access Stack B"]

    %% Infrastructure
    SERVERS["VMware Hosts / SAN"]
    WLAN["Wireless Infrastructure"]

    %% ISP Connections
    ISP1 -->|WAN| FW1
    ISP2 -->|WAN| FW1

    %% HA Sync
    FW1 ---|HA Sync| FW2

    %% Firewall to Core (Redundant 10Gb)
    FW1 -->|10Gb| CORE1
    FW1 -->|10Gb| CORE2
    FW2 -->|10Gb| CORE1
    FW2 -->|10Gb| CORE2

    %% Core Interconnect (25Gb Backbone)
    CORE1 <-->|25Gb LAG| CORE2

    %% Core to Access (10Gb)
    CORE1 -->|10Gb| ACCESS1
    CORE2 -->|10Gb| ACCESS2

    %% Core to Servers (10Gb)
    CORE1 -->|10Gb| SERVERS
    CORE2 -->|10Gb| SERVERS

    %% Core to WLAN
    CORE1 -->|10Gb| WLAN
    CORE2 -->|10Gb| WLAN
