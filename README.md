# Home SOC Lab – Security Monitoring Environment

## Overview
This project simulates a Security Operations Center (SOC) environment for monitoring and analyzing network traffic and security events.

The lab is designed to replicate real-world SOC workflows including log collection, alert generation, and investigation.

---

## Architecture

The environment is built using:

- Proxmox (virtualization platform)
- pfSense (firewall & gateway)
- Suricata (IDS – inline mode)
- Wazuh (SIEM platform)
- Pi-hole (DNS monitoring & filtering)
- Tailscale (secure remote access)

---

## Log Sources

The SOC collects and analyzes logs from multiple sources:

- Firewall logs (pfSense)
- IDS alerts (Suricata)
- DNS logs (Pi-hole)
- Endpoint/system logs (Wazuh agents)

---

## Detection Use Cases

The following detection scenarios are implemented:

- Port scan detection
- Brute-force login attempts
- Suspicious DNS activity

---

## Investigation Workflow

Typical workflow followed:

1. Alert triggered (IDS/DNS/SIEM)
2. Log correlation across multiple sources
3. Validation of suspicious activity
4. Conclusion (false positive or confirmed threat)

---

## Skills Demonstrated

- SIEM monitoring (Wazuh)
- IDS analysis (Suricata)
- Log correlation across multiple sources
- Threat detection and investigation
- Network traffic analysis

---

## Screenshots

Yet to be uploaded...

---

## Future Improvements

- Advanced detection rules
- Threat intelligence integration
- Automated alerting
