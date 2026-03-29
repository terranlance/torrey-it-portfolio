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
