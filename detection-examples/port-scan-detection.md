# Port Scan Detection Investigation

## Scenario

A network scan was performed against a monitored device using Nmap to simulate reconnaissance activity.

## Detection

Suricata generated alerts indicating multiple connection attempts across sequential ports, consistent with port scanning behavior.

## Investigation

The alerts were ingested into Wazuh SIEM where additional logs from pfSense firewall were reviewed. The connection attempts were confirmed as a scanning activity targeting multiple ports.

## Conclusion

The activity was classified as reconnaissance behavior typical of port scanning techniques.
