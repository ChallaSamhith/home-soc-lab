# Port Scan Detection Investigation

## Incident Overview

A simulated reconnaissance activity was conducted in the lab environment to replicate a common attacker behavior observed during the initial stages of network intrusion attempts. The activity involved a TCP SYN port scan targeting a monitored host to identify exposed services and open ports.

## Attack Technique

The scan was performed using the following command:

nmap -sS -p 1-1000 <target-ip>

This technique attempts to discover open ports by sending SYN packets to multiple ports on the target system.

This behavior aligns with the MITRE ATT&CK technique:

T1046 – Network Service Discovery

## Detection

Suricata IDS detected abnormal connection attempts originating from a single source IP address targeting a large range of destination ports within a short time window.

Multiple alerts were generated indicating potential reconnaissance behavior consistent with port scanning.

These alerts were forwarded to Wazuh SIEM where they were ingested as security events.

## Investigation

The alerts were reviewed in the Wazuh dashboard. A cluster of Suricata IDS alerts appeared within a short time period associated with the same source IP address.

Firewall logs from pfSense were analyzed to verify the traffic pattern. The logs confirmed repeated TCP SYN packets targeting sequential ports on the destination host.

Packet behavior and timing indicated automated scanning activity rather than normal user traffic.

## Analysis

Port scanning is commonly used by attackers to enumerate services running on a host before attempting exploitation.

The sequential connection attempts and high frequency of SYN packets strongly indicated reconnaissance activity.

## Conclusion

The reconnaissance activity was successfully detected by Suricata IDS and investigated through correlated alerts in Wazuh SIEM and firewall logs from pfSense.

This investigation demonstrates how network monitoring and log correlation can identify early-stage attacker reconnaissance activity within a SOC monitoring environment.
