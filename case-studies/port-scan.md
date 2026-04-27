# Port Scan Detection

## Scenario
Multiple connection attempts detected from a single IP across multiple ports.

## Detection
Suricata generated alerts indicating port scanning behavior.

## Investigation
- Checked firewall logs (pfSense)
- Verified repeated connection attempts
- Correlated timestamps with IDS alerts

## Conclusion
Confirmed reconnaissance activity (port scan)
