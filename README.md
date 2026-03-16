# Home Security Operations Center (SOC)

## Overview

This project demonstrates a Home Security Operations Center (SOC) designed to monitor real network activity using SIEM and IDS technologies.

The environment collects logs from firewall, DNS, IDS, and endpoints to support security monitoring and investigation workflows similar to those used in a SOC environment.

## Technologies Used

Wazuh SIEM  
Suricata IDS  
pfSense Firewall  
Pi-hole DNS  
Linux  
Wireshark  

## Architecture

Network traffic from household devices passes through a pfSense firewall where Suricata IDS monitors traffic for suspicious activity.

Security logs from Suricata, pfSense firewall, and Pi-hole DNS are centralized into Wazuh SIEM for monitoring, alerting, and investigation.

## Log Sources

pfSense firewall logs  
Suricata IDS alerts  
Pi-hole DNS query logs  
Endpoint system logs

## Detection Investigations

Examples of security investigations performed in this environment include:

Port scan detection  
Brute-force authentication attempts  
Suspicious DNS query activity

## Investigation Workflow

1. Suricata IDS generates an alert for suspicious network activity.
2. The alert is forwarded to Wazuh SIEM.
3. Firewall and DNS logs are reviewed for additional context.
4. Events are correlated to determine the nature of the activity.
5. Alerts are investigated and documented.
