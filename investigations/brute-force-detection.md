# Brute Force Detection Investigation

## Incident Overview

Repeated authentication attempts were detected targeting a monitored service within the home network.

The activity was generated intentionally from a Kali Linux virtual machine running on a Windows laptop connected to the same network. The purpose was to validate detection capabilities and practice SOC-style investigation workflows using the Home SOC monitoring environment.

## Attack Technique

A brute-force authentication attempt was simulated by generating repeated login attempts against a monitored system.

Brute-force attacks attempt to gain unauthorized access by repeatedly trying different password combinations.

MITRE ATT&CK Technique:
T1110 – Brute Force

## Detection

Multiple authentication failures were recorded within a short time window.

Suricata IDS generated alerts indicating repeated connection attempts from the same source IP address targeting the authentication service.

These alerts were forwarded to Wazuh SIEM where they appeared as security events.

## Investigation

Alerts were reviewed within the Wazuh monitoring dashboard.

Multiple Suricata IDS alerts were associated with the same source IP address performing repeated authentication attempts.

Authentication logs from the target system showed multiple failed login events within a short period.

Firewall logs from pfSense confirmed repeated connection attempts from the same host.

Correlation of IDS alerts, authentication logs, and firewall logs confirmed the activity as brute-force behavior.

## Analysis

Brute-force attacks are commonly used by attackers attempting to gain unauthorized access to systems by guessing credentials.

The repeated login failures and connection attempts matched typical brute-force attack patterns.

## Conclusion

The brute-force activity was successfully detected through Suricata IDS alerts and investigated using log correlation within Wazuh SIEM and firewall logs from pfSense.
