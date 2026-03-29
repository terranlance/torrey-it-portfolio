## High-Level Topology

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
