# VMware iSCSI Multipathing Runbook (Public Version)

---

## Objective
Standardize and optimize iSCSI multipathing across all ESXi hosts:
- ESX-A
- ESX-B
- ESX-C

---

## Architecture Summary
- Dual 25Gb NICs per host (vmnic2, vmnic3)
- Dedicated iSCSI VLAN (101)
- SAN1 (dual-controller SAN, ALUA enabled)
- Active/Optimized (AO) and Active/Non-Optimized (ANO) paths

---

## Configuration Steps

### 1. Verify NICs
```bash
esxcli network nic list
```

Expected:
- vmnic2 -> 25Gb UP
- vmnic3 -> 25Gb UP

---

### 2. Verify VMkernel Binding
```bash
esxcli iscsi networkportal list
```

Expected:
- 2 VMKs (vmk1, vmk2)
- Each bound to one NIC
- Status: compliant

---

### 3. NIC Teaming Configuration

| Portgroup | Active | Unused |
|----------|--------|--------|
| iSCSI01  | vmnic3 | vmnic2 |
| iSCSI02  | vmnic2 | vmnic3 |

Notes:
- Do not configure standby adapters
- Ensure strict 1:1 mapping between VMK and NIC

---

### 4. Rescan Storage
```bash
esxcli storage core adapter rescan --all
esxcli iscsi adapter discovery rediscover -A vmhba64
```

---

### 5. Verify Sessions
```bash
esxcli iscsi session list
```

Expected:
- Multiple sessions established
- Connectivity across all available SAN paths

---

### 6. Verify Paths
```bash
esxcli storage core path list
```

Expected:
- Multiple paths per LUN
- Paths distributed across both SAN controllers

---

### 7. Enable Round Robin Path Selection
```bash
esxcli storage nmp device set --device <naa> --psp VMW_PSP_RR
```

---

### 8. Set IOPS Policy
```bash
esxcli storage nmp psp roundrobin deviceconfig set \
--type iops \
--iops 1 \
--device <naa>
```

---

## Validation
```bash
esxcli storage nmp device list
```

Expected:
- Path selection policy: VMW_PSP_RR
- Configuration: policy=iops,iops=1

---

## Final State

| Host  | Status   |
|-------|----------|
| ESX-A | Complete |
| ESX-B | Complete |
| ESX-C | Complete |

---

## Key Notes
- ALUA determines optimal versus non-optimal paths
- Working paths are not equal to total available paths
- Validate both session count and path distribution

---

## Outcome
- Full SAN path redundancy
- Active-active multipathing configuration
- Load distribution across all available paths
- Consistent configuration across cluster

---

## Common Pitfalls
- Multiple NICs assigned to a single VMkernel adapter
- Missing IOPS configuration for Round Robin
- Incomplete SAN path discovery

---

## Conclusion
All hosts are fully connected to the SAN fabric, load-balanced, and operating with optimized multipathing.


## Host Server | iSCSI Network

```mermaid
flowchart LR

%% Hosts (compute style)
ESXA([ESX-A])
ESXB([ESX-B])
ESXC([ESX-C])

%% Core Switches (network style)
CORE01[[CORE01]]
CORE02[[CORE02]]

%% Connections (25 GB/s)
ESXA -- "25 GB/s" --> CORE01
ESXA -- "25 GB/s" --> CORE02

ESXB -- "25 GB/s" --> CORE01
ESXB -- "25 GB/s" --> CORE02

ESXC -- "25 GB/s" --> CORE01
ESXC -- "25 GB/s" --> CORE02

%% Styling
classDef host fill:#1f2937,stroke:#60a5fa,color:#ffffff,stroke-width:2px
classDef core fill:#065f46,stroke:#34d399,color:#ffffff,stroke-width:2px

class ESXA,ESXB,ESXC host
class CORE01,CORE02 core
