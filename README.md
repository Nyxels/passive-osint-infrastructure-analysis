# passive-osint-infrastructure-analysis
# Passive OSINT & Threat Infrastructure Analysis

A structured, passive Open-Source Intelligence (OSINT) investigation and technical triage of a compromised social media campaign and its underlying redirection infrastructure.

## 📌 Project Overview

This repository documents the technical analysis of a malicious Traffic Direction System (TDS). The investigation was triggered by a compromised Instagram account distributing malicious tracking links. The core objective was to map the threat actor's infrastructure, evaluate exposure vectors, and derive actionable mitigation strategies using strictly passive OSINT methodology.

### Key Analytical Focus:
*   **Infrastructure Mapping:** Correlating domain assets, virtual hosts, and historic network footprints.
*   **Traffic Triage:** Deconstructing HTTP redirection mechanics and state-tracking cookie serialization.
*   **Automation & Tool Integration:** Utilizing programmatic OSINT frameworks for accelerated data gathering.

---

## 📊 Methodology & Tool Stack

To maintain operational security (OPSEC) and adhere to a strict passive footprint, no active scanning, exploitation, or intrusive network probes were executed.

*   **Reconnaissance & Footprinting:** `SpiderFoot` (automated asset discovery, DNS/IP correlation).
*   **Identity & Endpoint Verification:** `Holehe` (OSINT email/account utilization tracking).
*   **Network Intelligence:** `Shodan API` (historical port mapping, SSL certificate tracking).
*   **Data Parsing & Decoding:** `Python` (Base64 deserialization, timestamp mapping).

---

## 🔎 Repository Structure

```text
.
├── reports/                      # Detailed technical output artifacts
├── INCIDENT_ANALYSIS_REPORT.pdf  # Comprehensive Executive & Technical Report (PDF)
├── holehe_report.txt             # Endpoint / OSINT identity verification logs
├── SpiderFoot.csv                # Raw structured intelligence data from automated footprinting
```

---

## 🛠️ Key Technical Findings

### 1. HTTP Vector & Redirection Mechanics
The campaign utilizes an implicit tracking layer before routing users to benign endpoints to evade automated security sandboxes.
*   **Initial Request:** `GET /magic` via ephemeral `.shop` domain.
*   **Server Architecture:** Distributed `nginx` instance handling multi-tenant virtual hosting.
*   **Redirection Layer:** `HTTP/1.1 302 Found` shifting context back to a legitimate destination after injecting an stateful tracking cookie.

### 2. State & Payload Analysis
Deep parsing of the network response revealed a PHP-serialized tracking array encoded in Base64:
```php
a:4:{i:0;i:0;i:1;i:1;i:2;a:6:{i:0;i:0;i:1;i:0;i:2;i:0;i:3;i:0;i:4;i:0;i:5;i:1;}i:3;i:1785889995;}
```
*   **Interpretation:** The vector stores a structured boolean matrix representing client-side environment validation flags (e.g., Bot-Detection, Geo-Filtering, Mobile-Check) alongside absolute Unix epoch timestamps for campaign synchronicity.

### 3. Infrastructure Correlation
Passive DNS and SSL footprinting tied the rogue virtual host to a specific hosting provider (`LLC Smart Ape`, Moscow, ASN: `AS56694`). The shared IPv4 address infrastructure hosts both legitimate target sites and weaponized tracking proxies, indicating either a compromised shared-hosting environment or a dynamic reverse-proxy setup.

---

## 💡 Practical Takeaways & Mitigation

The compiled report details standard Incident Response (IR) operational workflows:
1.  **Identity Triage:** Session termination, token invalidation, and MFA enforcement strategies.
2.  **Infrastructure Abuse Reporting:** Standardized playbooks for filing automated registrars and ISP abuse notifications (e.g., Selectel / Smart Ape).

---

## 📖 Disclaimer
This project is for educational and defensive analysis purposes only. All investigations were conducted leveraging public APIs, standard HTTP header responses, and publicly archived data.
