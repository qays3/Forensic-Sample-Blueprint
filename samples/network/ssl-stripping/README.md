# ssl-stripping

## Scenario
An attacker in the middle (10.10.1.66) downgrades the victim's HTTPS to HTTP (SSL strip). The victim talks plain HTTP to the attacker, who relays to the real HTTPS site, so the victim's login is visible.

## Timeline (UTC+3)
```
10:00:00  Victim 10.10.1.50 loads bank.example.com over plain HTTP via the attacker
10:00:xx  Links rewritten to http://, so credentials go in the clear
```

## How to spot it
- A site that should be HTTPS is served over plain HTTP
- Links and form actions rewritten from https to http
- The victim never upgrades to TLS
- Requires the attacker to be in the path (after ARP or DHCP spoof)

## Questions
```
1. Victim?           10.10.1.50
2. Attacker?         10.10.1.66
3. Technique?         SSL stripping (HTTPS to HTTP)
4. What is exposed?  The login over cleartext HTTP
5. Prerequisite?     Man in the middle position
```

## MITRE ATT&CK
- T1557 Adversary in the Middle
- T1040 Network Sniffing

## Tools
Crafted SSL strip (scapy) over generated background traffic

## Signature
Forensic Sample Blueprint | qayssarayra.com
