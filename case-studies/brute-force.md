# Brute Force Detection — SOC Investigation

**Date:** April 2026  
**Severity:** High  
**Status:** Confirmed — Brute Force Attempt  
**Tools:** Wazuh SIEM · Windows Event Logs · Suricata IDS · pfSense

---

## Scenario

Repeated failed authentication attempts detected against a Windows 
endpoint on the LAN. High-frequency login failures from a single 
source IP within a short time window.

---

## Detection

Wazuh rule 60204 (Windows failed login — Event ID 4625) fired 
repeatedly. Frequency-based rule 100501 triggered after 5 failures 
within 60 seconds, generating a level 10 alert.

**Windows Event ID:** 4625 — An account failed to log on  
**Wazuh rule triggered:** Rule 100501 — Multiple Windows login 
failures  
**Alert level:** 10  
**Telegram notification:** Received within 30 seconds

---

## Investigation Steps

**Step 1 — Review Wazuh alert**
- Confirmed 5+ Event ID 4625 entries from same source IP 
  within 60-second window
- Account targeted: local Administrator account
- Logon type: 3 (Network logon)

**Step 2 — Correlate with Suricata**
- Queried Wazuh for Suricata alerts from same source IP
- No concurrent IDS alerts — attack used legitimate 
  authentication protocol (SMB), not exploit payload

**Step 3 — pfSense firewall correlation**
- Confirmed SMB traffic (port 445) from source IP in 
  pfSense filterlog
- High-frequency connection attempts consistent with 
  automated tool (Hydra pattern)

**Step 4 — Source IP investigation**
- Source IP within LAN range — internal device
- Checked Wazuh DHCP logs (pfSense) for device identity
- Confirmed test machine running Hydra brute force simulation

---

## Conclusion

Confirmed brute force attack simulation using Hydra against Windows 
SMB. True positive. Source confirmed as internal test device. 
Multi-source correlation (Windows Event Logs + pfSense + Suricata) 
validated the detection pipeline end-to-end.

**MITRE ATT&CK:** T1110.001 — Brute Force: Password Guessing

---

## Detection Rule Used

```xml
<rule id="100501" level="10" frequency="5" timeframe="60">
  <if_matched_sid>60204</if_matched_sid>
  <description>Multiple Windows login failures — 
  possible brute force from $(srcip)</description>
</rule>
```
