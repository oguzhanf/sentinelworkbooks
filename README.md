# Microsoft Sentinel Workbooks Collection

**Originally authored by:** Omar Al Homaidi ([@ohomaidi](https://github.com/ohomaidi))

A curated set of **20 Azure Sentinel workbooks** designed for SOC analysts and security teams. The collection is split into two categories: multi-vendor government/enterprise dashboards and Microsoft-native detection workbooks.

---

## Deploy to Azure

Deploy all 20 workbooks to your Sentinel workspace with a single click. Each workbook is prefixed with a 3-letter identifier (**GOV** for multi-vendor Gov Dashboards, **MSO** for Microsoft Only) so they are easy to find in your workbook gallery.

[![Deploy to Azure](https://aka.ms/deploytoazurebutton)](https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fraw.githubusercontent.com%2Foguzhanf%2Fsentinelworkbooks%2Fmaster%2Fazuredeploy.json)

> You only need to provide your **Log Analytics workspace name**. The deployment creates all 20 workbooks as shared resources in the selected resource group.

---

## Repository Structure

```
├── Gov Dashboards/                    # Multi-vendor infrastructure threat detection (10 workbooks)
└── Microsoft Only Data Sources/       # Microsoft Defender / Entra / O365 telemetry (10 workbooks)
```

---

## Gov Dashboards

Infrastructure-centric workbooks that correlate events across multi-vendor environments (Fortinet, Palo Alto, Cisco, Juniper, F5, InfoBlox, Ericsson/Huawei). Each workbook proves an attack chain — no alert in isolation.

| # | Workbook | Description | Key Data Sources |
|---|----------|-------------|------------------|
| 01 | **GOV - Perimeter Breach Detection** | Correlates firewall allow/deny events with downstream web server activity to confirm perimeter breaches | CommonSecurityLog, W3CIISLog |
| 02 | **GOV - Identity-to-Network Kill Chain** | Reconstructs NAC/RADIUS auth → Windows logon → server access chains; detects credential relay | CommonSecurityLog (Cisco ISE), SecurityEvent, Syslog |
| 03 | **GOV - Data Exfiltration Across Layers** | Multi-layer data loss detection across proxy, IIS, Entra audit, and Unix file activity | CommonSecurityLog (Squid), W3CIISLog, AuditLogs, Syslog |
| 04 | **GOV - Lateral Movement & Reconnaissance** | Detects internal movement via Windows logons, Unix auth, and firewall internal traffic | SecurityEvent, Syslog, CommonSecurityLog |
| 05 | **GOV - Web Application Attack Chain** | Correlates WAF blocks/allows with web server errors to detect successful exploitation | CommonSecurityLog (WAF), W3CIISLog, Syslog |
| 06 | **GOV - Infrastructure Availability vs Security** | Distinguishes DDoS/attacks from outages by correlating NMS device-down events with firewall spikes | Syslog, CommonSecurityLog, Heartbeat |
| 07 | **GOV - Privileged Account Abuse** | Monitors admin activity across Windows, Unix, network, and cloud within 30-minute windows | SecurityEvent, Syslog, CommonSecurityLog, AuditLogs |
| 08 | **GOV - DNS as an Attack Vector** | InfoBlox-centric detection: DNS resolution → firewall connection correlation for C2 confirmation | CommonSecurityLog (InfoBlox), Syslog |
| 09 | **GOV - Cloud & Hybrid Threat Correlation** | Detects cloud-pivoted attacks: Azure resource deletion → on-prem logon → firewall allow | AzureActivity, SecurityEvent, CommonSecurityLog, AuditLogs |
| 10 | **GOV - SOC Health & Coverage** | Tracks data ingestion health, agent connectivity, connector status, and ingestion latency | Heartbeat, SentinelHealth, CommonSecurityLog, Syslog |

---

## Microsoft Only Data Sources

Workbooks built exclusively on Microsoft Defender, Entra ID, Office 365, and Azure telemetry. Focused on unified entity investigation — following attack trails from users, devices, and emails.

| # | Workbook | Description | Key Data Sources | Version |
|---|----------|-------------|------------------|---------|
| 01 | **MSO - Unified Security Posture** | Executive view across MDE, Entra ID, Defender for Cloud | SecurityAlert, DeviceInfo | v2 |
| 02 | **MSO - Identity Attack Chain** | Traces Entra sign-ins → AD → endpoint logons with lateral movement detection | SigninLogs, AADNonInteractiveUserSignInLogs, AuditLogs | v1 |
| 03 | **MSO - Privileged Access & Admin Abuse** | Monitors PIM activations, Azure privileged ops, O365 admin activity, service principals | AuditLogs, AzureActivity, AADServicePrincipalSignInLogs | v1 |
| 04 | **MSO - Endpoint Compromise & Lateral Movement** | Full post-compromise chain: processes, connections, files, and lateral movement via MDE | AlertInfo, DeviceProcessEvents, DeviceNetworkEvents, DeviceFileEvents | v2 |
| 05 | **MSO - Endpoint Health & Coverage** | MDE deployment state, patch gaps, exposure levels, and sensor health | DeviceInfo, SecurityRecommendation, DeviceVulnerabilityAssessment | v3 |
| 06 | **MSO - Phishing to Compromise Chain** | Full chain from email delivery → URL clicks → ZAP actions → account/device compromise | EmailEvents, EmailUrlInfo, SigninLogs, SecurityAlert | v4 |
| 07 | **MSO - Data Exfiltration & Insider Risk** | Data theft via SharePoint/OneDrive, email forwarding, endpoint files, and cloud uploads | OfficeActivity, DeviceFileEvents, EmailEvents | v1 |
| 08 | **MSO - Cloud Threat & Azure Anomaly** | Azure control plane monitoring, service principal anomalies, privileged Entra operations | AzureActivity, AADServicePrincipalSignInLogs, AuditLogs | v2 |
| 09 | **MSO - SOC Incident Investigation Helper** | Parameter-driven unified entity investigation with user and device timelines | SecurityAlert, SigninLogs, AuditLogs, OfficeActivity, DeviceProcessEvents | v2 |
| 10 | **MSO - Threat Hunting Dashboard** | Proactive hunting: rare processes, LOLBin abuse, first-time-seen IPs, suspicious logons | DeviceProcessEvents, DeviceNetworkEvents, DeviceLogonEvents, DeviceFileEvents | v1 |

---

## Threat Coverage Matrix

| Threat Scenario | Gov Dashboards | Microsoft Only |
|----------------|:-:|:-:|
| Perimeter Breach (FW → Web) | ✅ | — |
| Identity Kill Chain | ✅ | ✅ |
| Data Exfiltration | ✅ | ✅ |
| Lateral Movement | ✅ | ✅ |
| Web Application Attacks | ✅ | — |
| Availability vs DDoS | ✅ | — |
| Privileged Account Abuse | ✅ | ✅ |
| DNS C2 Detection | ✅ | — |
| Cloud & Hybrid Pivot | ✅ | ✅ |
| Phishing Chain | — | ✅ |
| Endpoint Compromise | — | ✅ |
| SOC Health & Ops | ✅ | ✅ |

---

## Quick Start

### Which workbook should I use?

| I want to... | Use |
|--------------|-----|
| Investigate a user compromise | Microsoft WB-09 (Investigation Helper) |
| Hunt for unknown threats | Microsoft WB-10 (Threat Hunting) |
| Check if a phishing email led to compromise | Microsoft WB-06 (Phishing Chain) |
| Detect data leaving the network | Gov WB-03 or Microsoft WB-07 |
| Monitor privileged account misuse | Gov WB-07 or Microsoft WB-03 |
| Verify firewall breach reached a server | Gov WB-01 (Perimeter Breach) |
| Confirm DNS-based C2 channels | Gov WB-08 (DNS Attack) |
| Review SOC ingestion health | Gov WB-10 (SOC Health) |
| Assess endpoint fleet posture | Microsoft WB-05 (Endpoint Health) |

### Deployment

1. Open **Microsoft Sentinel** → **Workbooks** → **Add workbook**
2. Switch to **Advanced Editor** (code view `</>`)
3. Paste the contents of the desired `.json` file
4. Click **Apply** then **Save**

---

## Prerequisites

### Gov Dashboards
- Microsoft Sentinel workspace with **CommonSecurityLog** connector (CEF/Syslog)
- Applicable vendor log sources forwarding to the workspace (firewall, WAF, DNS, NMS)
- **W3CIISLog** collection enabled for web server workbooks
- **SecurityEvent** connector for Windows event correlation

### Microsoft Only Data Sources
- **Microsoft Defender for Endpoint** (MDE) with advanced hunting tables
- **Microsoft Entra ID** (P1/P2) sign-in and audit log connectors
- **Microsoft Defender for Office 365** for email-based workbooks
- **Azure Activity** connector for cloud workbooks
- **Office 365** connector for collaboration telemetry

---

## License

This project is provided as-is for security operations use.
