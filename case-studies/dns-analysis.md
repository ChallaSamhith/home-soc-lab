# Suspicious DNS Activity — SOC Investigation

**Date:** April 2026  
**Severity:** High  
**Status:** Confirmed — Malicious Domain Blocked  
**Tools:** Pi-hole · Wazuh SIEM · pfSense · Suricata IDS

---

## Scenario

Pi-hole DNS filter blocked repeated queries to a known malicious 
domain from a device on the LAN. High-frequency blocking pattern 
consistent with malware C2 beaconing behaviour.

---

## Detection

Pi-hole blocked DNS query to a domain on the OISD blocklist. 
Wazuh rule 100601 fired on the first block. After 5 blocks within 
120 seconds, frequency rule 100602 triggered a C2 beaconing alert.

**Wazuh rule triggered:** Rule 100602 — Repeated DNS block — 
possible malware C2 beaconing  
**Alert level:** 12  
**Telegram notification:** Received within 30 seconds

---

## Investigation Steps

**Step 1 — Review Pi-hole alert in Wazuh**
- Confirmed `gravity blocked` log entries in Pi-hole log
- Domain: flagged on OISD Full + URLhaus blocklists
- Source device IP identified from Pi-hole query log

**Step 2 — Query frequency analysis**
- Reviewed Pi-hole dashboard Query Log
- Same domain queried every 30 seconds — consistent with 
  automated beacon interval
- Pattern not consistent with human browsing behaviour

**Step 3 — Cross-reference with Suricata**
- Queried Wazuh for Suricata alerts from source device IP
- No exploit or malware payload alerts — device not 
  actively exploiting other hosts

**Step 4 — pfSense firewall check**
- DNS bypass attempt: checked if device tried direct IP 
  connection to C2 server (bypassing DNS)
- pfSense NAT redirect confirmed all DNS forced through 
  Pi-hole — no bypass possible
- No direct IP connections to known malicious ranges found

**Step 5 — Endpoint investigation**
- Source device was a test Android phone
- Used to simulate malware C2 beacon by repeatedly 
  querying a blocklisted domain

---

## Conclusion

DNS-based C2 beaconing simulation successfully detected and blocked 
at the DNS layer. Pi-hole blocked all queries before any connection 
was established. Wazuh frequency rule correctly identified the 
beaconing pattern. True positive. No escalation required.

**MITRE ATT&CK:** T1071.004 — Application Layer Protocol: DNS

---

## Detection Rules Used

```xml
<rule id="100601" level="10">
  <decoded_as>pihole_blocked</decoded_as>
  <description>Pi-hole blocked malicious domain: $(url)</description>
</rule>

<rule id="100602" level="12" frequency="5" timeframe="120">
  <if_matched_sid>100601</if_matched_sid>
  <description>Repeated DNS block — possible C2 beaconing</description>
</rule>
```
