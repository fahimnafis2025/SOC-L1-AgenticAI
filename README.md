# AI-Powered SOC Triage Lab: Automated Tier 1 Analyst Pipeline

An automated, self-hosted Tier 1 Security Operations Center (SOC) analyst pipeline that ingests security telemetry from a Windows endpoint, performs local LLM-based triage using :contentReference[oaicite:0]{index=0} running :contentReference[oaicite:1]{index=1}, and escalates verified threats to responders via :contentReference[oaicite:2]{index=2}.

The system integrates :contentReference[oaicite:3]{index=3} for detection and telemetry correlation and uses :contentReference[oaicite:4]{index=4} for orchestration.

A deterministic safety bypass mechanism ensures high-severity alerts bypass LLM evaluation to reduce hallucination risk and guarantee incident escalation integrity.

---

## 🔍 What It Does (In Brief)

This project simulates a real-world SOC environment by acting as an automated Tier 1 analyst:

- Polls security event logs from a Wazuh SIEM indexer every 50 seconds
- Filters security noise (retaining rule levels ≥ 3)
- Performs LLM-based triage on enriched telemetry (logs, commands, MITRE ATT&CK mappings)
- Preserves endpoint metadata (IP, hostname, timestamps) via custom JavaScript reconstruction logic
- Applies deterministic escalation rules:
  - Severity ≥ 5 → immediate Discord escalation (no LLM decision)
  - Severity < 5 → LLM triage and classification

---

## 🛠️ Stack Overview

| Component | Technology | Purpose |
|-----------|------------|--------|
| SIEM & Indexer | :contentReference[oaicite:5]{index=5} (Kali Linux) | Security event correlation, MITRE mapping |
| Endpoint Agent | Wazuh Agent + Sysmon (Windows) | Process, registry, and task monitoring |
| Orchestration | :contentReference[oaicite:6]{index=6} | Workflow automation and pipeline logic |
| Reasoning Engine | :contentReference[oaicite:7]{index=7} + :contentReference[oaicite:8]{index=8} | Offline AI triage and classification |
| Notifications | :contentReference[oaicite:9]{index=9} Webhooks | Incident reporting and alerting |

---

## 🚀 Capabilities

### Multi-Event Parallel Processing
- Batch processing (groups of 5 events)
- Prevents LLM context overload during alert spikes

### Deterministic Security Overrides
- “Golden Override Rule”
- Severity ≥ 5 bypasses AI reasoning entirely
- Ensures guaranteed escalation path

### Telemetry Preservation
- Reconstructs and preserves:
  - Hostname
  - Device IP
  - Event timestamps
  - Raw telemetry context

### Adversary Emulation Suite
Non-destructive PowerShell-based simulations:
- Registry persistence (Run keys)
- Scheduled task persistence
- DNS beaconing simulation
- File staging / compression
- Process injection chains (mock behavior)

---

## 🛠️ What I Did & How I Did It

### 1. SIEM & Infrastructure Engineering
- Deployed Wazuh Manager on Kali Linux VM (VirtualBox)
- Configured dual NICs:
  - NAT (internet access)
  - Host-only (internal communication)
- Fixed Host-Only IPv4 binding issues
- Reinstalled Wazuh + OpenSearch after corruption
- Reconfigured OpenSearch (`opensearch.yml`):
  - Changed binding from `127.0.0.1` → `0.0.0.0`
  - Enabled external access for n8n integration

---

### 2. Endpoint Provisioning & Simulation
- Installed Wazuh agent on Windows endpoint
- Built MITRE-aligned simulation scripts:
  - T1053.005: Scheduled tasks persistence
  - T1547.001: Registry run keys
  - T1071: DNS beaconing simulation
  - T1560: Data staging/compression
  - T1055: Process injection mimicry

---

### 3. Pipeline Orchestration (n8n)
- Built full event-driven workflow in :contentReference[oaicite:10]{index=10}
- Implemented:
  - Polling triggers
  - REST API integration (Wazuh Manager API)
  - JWT authentication flow (port 55000)
  - JavaScript-based alert parsing
  - Batch iteration processing logic

---

### 4. Local AI Engineering
- Replaced rate-limited cloud LLM (Gemini API) with fully offline stack:
  - :contentReference[oaicite:11]{index=11}
  - :contentReference[oaicite:12]{index=12} (3B)

Key fixes:
- Resolved JSON schema deserialization errors in n8n
- Enforced strict boolean casting for API compatibility
- Designed structured system prompt forcing JSON output:
  - verdict
  - confidence
  - reasoning
  - recommended_action

---

## 📂 Project Structure
