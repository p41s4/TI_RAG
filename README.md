# RAG – CTI Automation Workflow

## General Description

This repository contains a **Threat Intelligence (CTI) automation workflow** developed in **n8n**, designed to analyze **Indicators of Compromise (IoCs)** and generate **automatic intelligence reports** by combining multiple CTI sources with a **Language Model (LLM)**.

The workflow receives an indicator (IP, hash, or URL), queries **VirusTotal** and **AlienVault OTX**, normalizes and merges the results, and finally generates an **automatically written intelligence report** using an AI agent.

<img width="1272" height="408" alt="image" src="https://github.com/user-attachments/assets/1f84408e-a2b4-49a8-8b1a-99f6b6019351" />

---

## What is it for?

This workflow is used to:

- Automate the analysis of **IPs, hashes, and URLs**.
- Centralize information from **multiple Threat Intelligence sources**.
- Reduce manual analysis time in SOC environments.
- Generate **clear, structured, and actionable reports**.
- Serve as a foundation for a **RAG (Retrieval-Augmented Generation)** system applied to cybersecurity.

Common use cases:
- Security Operations Center (SOC)
- Threat Hunting
- Incident Response
- Automatic alert enrichment
- Rapid analysis of IoCs received via email, tickets, or chat

---

## Workflow (high level)

1. **Indicator intake**
   - The workflow is triggered when a chat message is received.
   - It automatically detects whether the input is:
     - IP address
     - Hash (MD5, SHA1, or SHA256)
     - URL

2. **IoC normalization**
   - The indicator type is identified.
   - Correct API paths are built for:
     - VirusTotal API
     - AlienVault OTX API
   - For URLs, the Base64 ID required by VirusTotal is generated.

3. **CTI source queries**
   - **VirusTotal**
     - Engine detections
     - Reputation and community votes
     - Geolocation, ASN, and network data
     - Timestamps (first seen, last seen, analysis)
     - Tags, categories, and threat severity
   - **AlienVault OTX**
     - Related pulses
     - Associated malware and adversaries
     - Targeted industries
     - Public references

4. **Results merging**
   - Responses from both sources are combined.
   - No heuristics or artificial scoring are applied.

5. **Post-processing (Code Node)**
   - Generates:
     - A structured JSON `summary`
     - A compact `ai_brief` optimized for AI usage

6. **Report generation**
   - An **AI Agent** automatically writes a Threat Intelligence report in natural language.

---

## Workflow output

The final result includes:

- Indicator type and analyzed value.
- Structured summaries from:
  - VirusTotal
  - AlienVault OTX
- Automatically generated CTI report, ready for:
  - Incident tickets
  - Internal reports
  - Sharing with other teams

---

## Main components

- **Chat Trigger** – Indicator input.
- **Code Nodes (JavaScript)**
  - IoC detection and normalization.
  - CTI data reduction, cleanup, and unification.
- **HTTP Request Nodes**
  - VirusTotal API
  - AlienVault OTX API
- **Merge Node** – Result consolidation.
- **AI Agent + OpenAI Model** – Automatic report generation.

---

## Requirements

- A functional **n8n** instance.
- Valid API credentials for:
  - VirusTotal
  - AlienVault OTX
  - OpenAI
- Basic knowledge of:
  - Threat Intelligence
  - n8n
  - Indicators of Compromise

---

## Possible extensions

- Persistence to a database or SIEM.
- Risk scoring calculation.
- Integration with Slack, Jira, or ServiceNow.
- Additional enrichment (WHOIS, Shodan, AbuseIPDB).
- Use as a backend for a SOC chatbot or a full RAG platform.

---

## License

Review the terms of use of VirusTotal, AlienVault OTX, and OpenAI before using this workflow in production.
