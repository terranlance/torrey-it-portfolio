```mermaid

flowchart LR

%% =========================
%% ZONES
%% =========================

subgraph PROD["Production Network"]
    HospitalCore["Hospital Core"]
    HospitalFW["Hospital Firewall"]
end

subgraph ANALYSIS["Analysis Zone"]
    SpanPort["SPAN / Mirror Port"]
    Janice["Janice\nTraffic Analyzer"]
end

subgraph LAB["Compute & Storage Environment"]
    SocFW["SOC Firewall"]
    ComputeStorage["Compute and Storage"]
    GUPPI["GUPPI\nAI / LLM Workloads"]
end

subgraph INTERNET["Internet"]
    ISP["ISP"]
end

%% =========================
%% FLOWS
%% =========================

HospitalFW -->|Mirrored Traffic| SpanPort
SpanPort -->|Passive Analysis| Janice

HospitalCore -->|Transit Network| SocFW
SocFW --> ComputeStorage
ComputeStorage --> GUPPI

SocFW -->|Controlled Access| HospitalCore
SocFW -->|Independent WAN| ISP
