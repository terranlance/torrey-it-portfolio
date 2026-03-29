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
