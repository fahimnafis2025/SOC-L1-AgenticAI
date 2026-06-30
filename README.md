# AI-Powered SOC Triage Lab

## What it is

An automated Tier 1 SOC triage pipeline that processes security alerts from a Wazuh SIEM, applies rule-based escalation, and uses a local LLM (Llama 3.2 via Ollama) to classify and triage lower-severity events. High-severity alerts bypass the model and are directly escalated to responders through Discord.

---

## What it does

- Collects security telemetry from a Wazuh-managed endpoint environment
- Filters and prioritizes alerts based on severity level
- Sends medium and low severity events to a local LLM for classification
- Automatically escalates high severity events without AI involvement
- Sends structured incident reports to Discord for response actions
- Preserves full endpoint context (host, IP, timestamp, process activity)

---

## How it was built

The system was built as a modular SOC automation lab using virtualized infrastructure and open-source tooling.

- Wazuh manager deployed on a Kali Linux virtual machine
- Windows endpoint configured with Wazuh agent and Sysmon for telemetry generation
- OpenSearch configured for external access to allow workflow integration
- n8n used as the central orchestration engine for polling, routing, and processing alerts
- Custom JavaScript functions used to parse and reconstruct alert payloads
- Discord webhooks used for incident notification delivery
- Local LLM inference implemented using Ollama running Llama 3.2

---

## Tools and technologies used

- Wazuh SIEM (log collection and correlation)
- Sysmon (Windows endpoint telemetry)
- OpenSearch (alert indexing and querying)
- n8n (workflow automation and orchestration)
- Ollama (local LLM runtime)
- Llama 3.2 (alert reasoning and classification model)
- Discord webhooks (alert notification channel)
- Virtualization platform (VirtualBox or equivalent lab setup)

---

## How triage works

The triage process follows a deterministic + AI hybrid model:

1. Alerts are pulled from Wazuh at fixed intervals
2. Each alert is assigned or evaluated for severity
3. If severity is 5 or higher:
   - Alert bypasses the LLM
   - Immediately forwarded to Discord as a critical incident
4. If severity is below 5:
   - Alert is sent to Llama 3.2 via Ollama
   - Model returns structured JSON output including:
     - verdict
     - confidence
     - reasoning
     - recommended action
5. Final enriched alert is sent to responders via Discord

This ensures critical events are never delayed or misclassified by the model.

---

## How it can help SOCs

- Reduces Tier 1 analyst workload by automating repetitive alert triage
- Filters noise from high-volume SIEM environments
- Provides consistent initial classification for low and medium severity events
- Ensures fast escalation of critical incidents through deterministic rules
- Improves response time by reducing manual alert review overhead

---

## Business impact

This system helps organizations reduce operational cost in security monitoring by automating early-stage alert handling. It improves SOC efficiency, reduces analyst fatigue, and increases the speed of incident detection and escalation without relying on external AI APIs.

---

## What can be customized

This system is modular and can be adapted to different SOC environments, toolchains, and threat models. The following components can be modified:

### 1. SIEM and log source
- Replace Wazuh with other SIEM platforms (Splunk, Elastic Security, Sentinel)
- Change log ingestion frequency and filters
- Adjust rule severity mapping logic

### 2. Endpoint telemetry
- Swap Sysmon with other endpoint agents or EDR tools
- Modify which event types are collected (process, registry, network, file activity)
- Add or remove MITRE ATT&CK simulation scenarios

### 3. Workflow orchestration
- Replace n8n with alternatives like Node-RED, Apache Airflow, or custom Python pipelines
- Adjust polling intervals and batch sizes
- Modify routing logic for multi-tenant SOC environments

### 4. LLM and reasoning layer
- Replace Ollama with other local or cloud models
- Swap Llama 3.2 with different models (Mistral, Qwen, Llama 3.1, etc.)
- Change prompt structure and JSON output schema
- Enable or disable AI triage entirely

### 5. Alert routing and notifications
- Replace Discord with Slack, Teams, email, or SIEM ticketing systems (ServiceNow, Jira)
- Modify alert formatting and enrichment fields
- Add escalation workflows for different severity tiers

### 6. Security logic and triage rules
- Change severity threshold for bypass logic
- Add custom detection rules or heuristic scoring
- Introduce multi-stage escalation (Tier 1 → Tier 2 → IR team)

### 7. Infrastructure setup
- Run components on Docker instead of VirtualBox
- Deploy on cloud VMs instead of local lab machines
- Split architecture into distributed services for scaling

---

## Summary

This project demonstrates a flexible SOC automation pipeline combining SIEM telemetry, workflow automation, and local AI reasoning. It can be adapted to different environments by modifying the SIEM source, orchestration layer, model backend, and alert routing logic.
