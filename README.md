# AI-Powered SOC Triage Lab

## What it is

An automated Tier 1 SOC triage system that processes security telemetry from a Wazuh SIEM, applies structured filtering, and uses a local LLM (Llama 3.2 via Ollama) to classify alerts as true positives or false positives. High-severity incidents bypass the model and are escalated immediately to responders via Discord.

The system is designed to simulate a real SOC Tier 1 analyst workflow with deterministic controls and AI-assisted decision support.

---

## What it does

- Ingests security alerts from a Wazuh SIEM environment
- Filters alerts based on rule level (only rule level ≥ 3 is processed)
- Routes alerts through a triage pipeline based on severity
- Uses a local LLM to classify lower severity alerts
- Escalates high severity alerts immediately without AI processing
- Sends structured incident reports to Discord for response

Core output includes:
- True positive or false positive classification
- Confidence score
- Reasoning
- Recommended action
- Full endpoint telemetry context

---

## How it was built

The system was built as a modular SOC automation lab using open-source security and orchestration tools.

- Wazuh deployed on a Kali Linux virtual machine for SIEM and log correlation
- Windows endpoint configured with Wazuh agent and Sysmon for telemetry generation
- OpenSearch configured for external access and alert querying
- n8n used as the orchestration engine for polling, routing, and processing alerts
- JavaScript-based logic used for parsing and reconstructing alert payloads
- Ollama used for local LLM inference with Llama 3.2
- Discord webhooks used for alert delivery and incident reporting

---

## Tools and technologies used

- Wazuh SIEM (security event collection and correlation)
- Sysmon (Windows endpoint telemetry)
- OpenSearch (log indexing and querying)
- n8n (workflow automation and orchestration)
- Ollama (local LLM runtime)
- Llama 3.2 (classification and reasoning model)
- Discord webhooks (incident notification system)
- VirtualBox or equivalent virtualization platform

---

## How triage works

The pipeline uses deterministic filtering combined with LLM-based threat validation.

### 1. Alert filtering
- Alerts are pulled from Wazuh at scheduled intervals
- Only alerts with rule level ≥ 3 are processed
- Lower-level noise is discarded before analysis

### 2. LLM-based triage (all alerts)
- All remaining alerts, including high severity ones, are sent to Llama 3.2 via Ollama
- The LLM evaluates whether each alert represents a legitimate security incident

The model determines:
- True positive or false positive classification
- Threat reasoning based on telemetry context
- Confidence score
- Recommended interpretation of the event

### 3. Escalation logic
- If the LLM classifies the alert as a true positive:
  - The alert is escalated to Discord as an incident
- If the LLM classifies the alert as a false positive:
  - The alert is discarded and not forwarded

### 4. Final output
Only alerts confirmed as true positives by the LLM are sent to responders via Discord

---

## How it can help SOCs

- Reduces Tier 1 analyst workload by automating initial alert triage
- Filters noise before human review
- Provides consistent classification of security events
- Improves response time for critical incidents
- Standardizes alert enrichment and reporting format

---

## Business impact

This system reduces operational cost in security monitoring by automating repetitive Tier 1 tasks. It improves SOC efficiency, reduces analyst fatigue, and enables faster incident detection and escalation while maintaining deterministic control over high-severity events.

---

## What can be customized

This system is modular and can be adapted to different environments and toolchains.

### SIEM and data sources
- Replace Wazuh with Splunk, Elastic Security, Microsoft Sentinel, or other SIEM platforms
- Modify rule level filtering thresholds
- Adjust alert ingestion frequency and query logic

### Endpoint telemetry
- Replace Sysmon with alternative EDR or endpoint agents
- Modify collected event types (process, registry, network, file activity)
- Add or remove MITRE ATT&CK simulation scenarios

### Workflow orchestration
- Replace n8n with Node-RED, Apache Airflow, or custom scripts
- Adjust batch sizes and polling intervals
- Modify routing logic for different SOC tiers

### LLM and reasoning layer
- Replace Ollama with cloud or other local inference engines
- Swap Llama 3.2 with other models (Mistral, Qwen, Llama 3.1, etc.)
- Modify prompt structure and output schema
- Disable LLM layer entirely for deterministic-only mode

### Alert routing and notifications
- Replace Discord with Slack, Microsoft Teams, email, or ticketing systems (ServiceNow, Jira)
- Customize incident formatting and enrichment fields
- Add multi-channel escalation workflows

### Security logic
- Adjust severity threshold for bypass logic
- Modify true positive vs false positive classification rules
- Add multi-stage escalation (Tier 1 → Tier 2 → Incident Response)

### Infrastructure setup
- Move from VirtualBox to Docker-based deployment
- Deploy components on cloud infrastructure
- Split pipeline into distributed services for scaling

---

## Summary

This project demonstrates a practical SOC automation pipeline combining SIEM telemetry, workflow orchestration, and local AI reasoning. It focuses on structured triage, deterministic escalation for critical events, and flexible AI-assisted classification for lower severity alerts.
