# Port Scan Detection — SOC Investigation

**Date:** April 2026  
**Severity:** Medium  
**Status:** Confirmed — Reconnaissance Activity  
**Tools:** Suricata IDS · Wazuh SIEM · pfSense firewall logs · Wireshark

---

## Scenario

Multiple rapid connection attempts detected from a single internal IP 
address across a wide range of ports on a target host within the LAN.

---

## Detection

Suricata IDS generated an `ET SCAN` alert after detecting SYN packets 
to 1,000+ ports within a 60-second window. Alert written to eve.json 
and forwarded to Wazuh SIEM via the Wazuh agent.

**Wazuh rule triggered:** Rule 100801 — Suricata: Port scan or 
network reconnaissance detected  
**Alert level:** 10  
**Telegram notification:** Received within 30 seconds

---

## Investigation Steps

**Step 1 — Validate the Suricata alert**
- Reviewed eve.json alert: `event_type: alert`, 
  `alert.category: Attempted Information Leak`
- Confirmed source IP and destination IP from alert fields

**Step 2 — Correlate with pfSense firewall logs**
- Queried Wazuh Discover for `agent.name:pfSense` + source IP
- pfSense filterlog confirmed high-frequency outbound SYN packets
- Timestamps aligned with Suricata alert window

**Step 3 — Packet analysis in Wireshark**
- Captured traffic on Suricata br0 interface during resimulation
- Confirmed SYN-only packets with no corresponding SYN-ACK 
  (classic stealth scan pattern)
- Source port randomised — consistent with Nmap -sS behaviour

**Step 4 — Endpoint check**
- Queried Wazuh for Windows Sysmon events from source IP
- Identified `nmap.exe` process creation event (Event ID 1)
- Process launched from user account — not malware

---

## Conclusion

Confirmed internal port scan originating from a known device running 
Nmap. No external threat — internal reconnaissance test. Alert 
correctly classified as true positive. No escalation required.

**MITRE ATT&CK:** T1046 — Network Service Discovery

---

## Detection Rule Used

```xml
<rule id="100801" level="10">
  <decoded_as>json</decoded_as>
  <field name="event_type">alert</field>
  <field name="alert.category">Attempted Information Leak</field>
  <description>Suricata: Port scan detected from $(src_ip)</description>
</rule>
```
