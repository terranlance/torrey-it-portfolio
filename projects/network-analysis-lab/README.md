# Network Analysis Lab & Segmented Architecture

## Overview

As part of a broader Tier 3 network redesign initiative, a dedicated lab environment was migrated into the hospital infrastructure to serve two primary purposes:

1. Establish a **baseline of network behavior** prior to major architectural changes  
2. Provide a **safe, isolated environment** for traffic analysis and AI-driven experimentation  

What began as a simple lab migration evolved into a more structured design focused on **segmentation, observability, and controlled integration with production systems**.

---

## Objectives

- Capture **baseline network traffic** before Tier 3 redesign
- Enable **passive traffic analysis** via SPAN/mirror port
- Maintain **strict isolation** from production systems
- Provide **controlled access** to select hospital resources
- Support **AI/LLM workloads (GUPPI)** without impacting core infrastructure
- Avoid re-IP or disruption of the existing lab environment during migration

---

## Architecture Approach

The solution separates the environment into three distinct zones:

### 1. Analysis Zone
- Receives traffic from a **SPAN / mirror port** on the production firewall
- Designed for **passive observation only**
- No IP assignment, no routing, no ability to transmit traffic back into production
- Supports tools such as packet capture, flow analysis, and future AI-driven inspection ("Janice")

---

### 2. Proxmox Compute & Storage (Dupras)

A self-contained compute environment consisting of:

- Proxmox host infrastructure
- Storage (Nimble-backed iSCSI)
- AI/LLM workloads (GUPPI)

This environment:
- Retains its original **172.16.x.x addressing**
- Is fully contained behind a dedicated firewall boundary
- Supports experimentation and analysis without impacting production

---

### 3. Production Network (Hospital)

- Existing hospital network (`172.19.x.x`)
- Core infrastructure and services
- Protected by primary firewall infrastructure

Interaction with the lab environment is:
- **Explicitly controlled**
- Limited to required services only
- Routed through a dedicated transit network

---

## Network Design Principles

### Segmentation First
Each traffic type is treated independently:

- Analysis traffic → **one-way, isolated**
- Lab traffic → **contained behind firewall**
- Production access → **explicitly allowed only where required**
- Internet access → **independent from hospital network**

---

### Controlled Integration

A **transit network** connects the lab firewall to the hospital network:

- Enables selective communication
- Prevents full network blending
- Supports NAT or routed access as needed

---

### Baseline Before Change

A key driver of this design was the need to:

> Understand current network behavior before introducing major architectural changes

This allows:
- Meaningful comparison after Tier 3 implementation
- Better troubleshooting and validation
- Data-driven decision making

---

## Architecture Diagram

![Network Architecture](./diagram.png)

---

## Key Takeaways

- Not all network connections should be treated equally  
- Passive analysis environments must remain fully isolated  
- Segmentation simplifies both **security** and **operations**  
- Establishing a **baseline before change** is critical for meaningful improvement  
- Small lab environments can evolve into powerful **observability platforms**  

---

## Future Enhancements

- Integration of network analysis tools (Zeek, Suricata, ELK stack)
- AI-assisted anomaly detection using GUPPI
- Expanded telemetry collection and visualization
- Potential evolution toward a lightweight SOC capability

---

## Related Projects

- Tier 3 Network Upgrade (in progress)
- Firewall High Availability Migration
- Infrastructure Documentation & Runbooks

---
