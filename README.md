# Home Security Operations Center (SOC)

## Overview

This project demonstrates a Home Security Operations Center (SOC) designed to monitor real network activity using SIEM and IDS technologies.

The environment collects logs from firewall, DNS, IDS, and endpoints to support security monitoring and investigation workflows similar to those used in a SOC environment.

---

## Technologies Used

- Wazuh SIEM
- Suricata IDS
- pfSense Firewall
- Pi-hole DNS
- Linux
- Wireshark

---

## Architecture

Network traffic from household devices is monitored through a pfSense firewall. Suricata IDS analyzes network traffic and generates intrusion alerts. Logs from Suricata, pfSense, DNS activity, and endpoints are centralized in Wazuh SIEM for monitoring and alert correlation.

---

## Log Sources

- Firewall logs from pfSense
- IDS alerts from Suricata
- DNS query logs from Pi-hole
- Endpoint logs from Linux and Windows systems

---

## Detection Use Cases

### Port Scan Detection

Suricata detects network reconnaissance activity such as port scans performed using tools like Nmap. Alerts are generated and forwarded to Wazuh SIEM for investigation.

### Brute Force Detection

Repeated authentication attempts are monitored through IDS alerts and log analysis to identify potential brute-force activity.

### Suspicious DNS Queries

DNS query logs from Pi-hole are analyzed to identify connections to potentially suspicious or unusual domains.

---

## Investigation Workflow

1. Suricata generates an IDS alert for suspicious activity.
2. The alert is forwarded to Wazuh SIEM.
3. Additional logs from firewall and DNS sources are reviewed.
4. Events are correlated to determine the nature of the activity.
5. Alerts are investigated and documented.

---

## Project Goal

The objective of this project is to simulate a SOC monitoring environment where security alerts can be detected, investigated, and analyzed using multiple telemetry sources.
