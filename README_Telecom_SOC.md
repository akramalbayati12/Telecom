# Telecom SOC — Threat Detection & Security Analysis

A Splunk-based Security Operations Center (SOC) project focused on threat detection, cross-source correlation and incident analysis in a simulated telecommunications environment.

**Author:** Akram Al-Bayati  
**Date:** November 2026  
**Platform:** Splunk  
**Framework:** MITRE ATT&CK  
**Splunk index:** `telecom_soc`

---

## Project Overview

This project builds a telecom-focused SOC monitoring and detection workflow using six categories of carrier-style telemetry. Fifteen detection use cases were designed and mapped to MITRE ATT&CK, with their results presented in a dark-theme **Telecom SOC // Threat Operations Center** dashboard and incident register.

The project investigates threats relevant to telecommunications environments, including attacks against AAA authentication, SIP/IMS services, DNS, VPN access, network flows and subscriber-management processes.

> **Dataset note:** The project uses a synthetic training dataset generated to resemble carrier telemetry. It is not data from a real telecommunications operator.

## Project at a Glance

| Item | Details |
|---|---|
| Detection catalogue | 15 detections (`DET-01` to `DET-15`) |
| Log sources | 6 |
| Splunk index | `telecom_soc` |
| Dataset period | November 2026 |
| Severity levels | Critical, High, Medium |
| Dashboard | Telecom SOC // Threat Operations Center |
| Analysis | SPL detection, MITRE ATT&CK mapping and cross-source correlation |

## Log Sources

Six telemetry sources are ingested into the `telecom_soc` index:

1. **AAA authentication** — authentication outcomes and account activity.
2. **Firewall** — allowed and denied network traffic.
3. **DNS** — domain queries and DNS record activity.
4. **Network flows** — connection and data-transfer metadata.
5. **VPN** — remote-access and MFA activity.
6. **Subscriber events** — subscriber-account changes and SIM-related activity.

## Detection Catalogue

The project defines fifteen detections:

| ID | Detection |
|---|---|
| DET-01 | Brute Force: 10 or More Failures in 30 Minutes |
| DET-02 | Brute Force Followed by Successful Authentication |
| DET-03 | Password Spraying from an External IP |
| DET-04 | Cleartext PAP Success from an External IP |
| DET-05 | UDP Flood Against SIP/IMS (Port 5060) |
| DET-06 | Port Scan: Many Ports on the Same Target |
| DET-07 | External Source Allowed to Management Ports |
| DET-08 | DNS Tunneling: Long Queries and TXT Records |
| DET-09 | Many Unique Subdomains under One Parent Domain |
| DET-10 | High-Risk Subscriber Changes Pending Review |
| DET-11 | Large Outbound Transfer from the Internet Zone |
| DET-12 | Rare External Destination Contacted by One Host |
| DET-13 | Repeated VPN MFA Failures (MFA Fatigue) |
| DET-14 | Same External IP Across AAA, Firewall and VPN |
| DET-15 | SIM Swap Followed by Password Reset (within 60 Minutes) |

The report documents fourteen detections with validation screenshots. **DET-02 is included in the detection catalogue and dashboard, but its failure-then-success correlation is identified as a tuning item:** the dashboard's Compromised Accounts tile showed zero in the documented run, while the incident register contained a planted scenario.

## Dashboard

The **Telecom SOC // Threat Operations Center** consolidates detection and monitoring information in a dark-theme Splunk dashboard.

Its views include:

- KPI tiles and authentication overview
- AAA authentication detections
- PAP and firewall analytics
- DNS, network-flow and VPN monitoring
- Subscriber-fraud analysis
- Cross-source IP correlation
- Incident register and recommended responses
- Detection coverage and MITRE ATT&CK mapping

A geo-map panel is included as a dashboard slot; enabling its geolocation layer requires the configuration described in the report.

## Key Findings

The report describes a coordinated campaign involving five recurring external IP addresses across multiple detection categories:

- `103.75.201.9`
- `185.220.101.14`
- `194.26.29.77`
- `45.155.205.10`
- `91.240.118.22`

Other notable findings include:

- **Authentication:** 723 authentication failures and five external brute-force source IPs.
- **Firewall:** 2,928 blocked firewall events.
- **SIP/IMS:** a UDP flood targeting port 5060, denied at the network edge.
- **DNS:** one internal host generated 180 unique TXT subdomains associated with the tunnelling scenario.
- **Data transfer:** an internal host transferred approximately 574 MB to a rare external destination.
- **Subscriber security:** 51 high-risk subscriber changes were pending review, and SIM-swap/password-reset scenarios were identified.
- **Remote access:** the report highlights external exposure of management services and repeated VPN MFA failures.

These findings are based on the simulated dataset and its planted scenarios.

## Methodology

Each detection follows a repeatable lifecycle:

1. **Hypothesis** — define the attacker behaviour and relevant log source.
2. **Query** — write SPL using filtering, time windows, aggregation and thresholds.
3. **Validate** — run the query in Splunk and compare the results with the expected scenario.
4. **Classify** — map the behaviour to MITRE ATT&CK and assign severity.
5. **Visualise and respond** — display the finding on the dashboard with a recommended analyst response.

Claude and ChatGPT assisted with SPL drafting, threshold selection, MITRE mapping and documentation. The analyst executed the searches, inspected their results and adjusted the logic.

## Running the Project

Use the project data and any accompanying Splunk assets with a Splunk environment.

1. Prepare the synthetic telecom dataset while preserving the original event fields.
2. Ingest the six log sources into the Splunk index `telecom_soc`.
3. Open **Search & Reporting** and select a time range covering November 2026 (or All time for the full lab dataset).
4. Run the detection searches against `index=telecom_soc`.
5. Import or recreate the dashboard, ensuring that its searches reference the same index and field names.
6. Review the detection outputs alongside the incident register and report evidence.

> **Important:** This README describes the project as documented. It does not claim that raw dataset files, dashboard XML, or saved-search exports are included unless they are committed to the repository separately.

## Limitations and Future Work

The dataset is synthetic and contains planted scenarios. Detection thresholds must be tuned against real, anonymised carrier telemetry before production use.

The report identifies these next steps:

- Tune DET-02's failure-then-success correlation and validate it against incident `INC-001`.
- Enable the dashboard geolocation layer and enrich IP addresses with threat-intelligence and ASN data.
- Convert detections into scheduled Splunk alerts and connect them to SOAR playbooks.
- Test against real, anonymised carrier telemetry and establish operational baselines.
- Add detections for SS7/Diameter signalling abuse and 5G Service-Based Architecture (SBA) anomalies.

## Disclaimer

This project is intended for defensive cybersecurity education and portfolio demonstration. The dataset is synthetic; IP addresses, subscriber identifiers and domains are simulated. Findings do not describe a real carrier or real subscriber activity.

---

**Author:** Akram Al-Bayati  
**Focus:** SOC Analysis · Threat Detection · Splunk SPL · MITRE ATT&CK · Telecom Security
