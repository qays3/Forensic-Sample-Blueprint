# exfil-to-paste-site

## Scenario
A host (10.10.1.62) posts stolen data to a public paste site (pastebin.com). The attacker later reads it from the raw link, so the data leaves through a trusted, common service.

## Timeline (UTC+3)
```
14:00:00  10.10.1.62 POSTs to pastebin.com /api/api_post.php
14:00:xx  Response returns a raw paste URL
```

## How to spot it
- A POST to a paste site API from a workstation
- Data leaves through a trusted public service
- The response hands back a paste URL the attacker will read
- Uncommon for business workflows

## Questions
```
1. Host?             10.10.1.62
2. Service?          pastebin.com
3. Endpoint?         /api/api_post.php
4. What returns?     A raw paste URL
5. Why used?         Trusted service hides the exfil
```

## MITRE ATT&CK
- T1567.001 Exfiltration to Code Repository / Paste Site
- T1048 Exfiltration Over Alternative Protocol

## Tools
Crafted paste site exfil (scapy) over generated background traffic

## Signature
Forensic Sample Blueprint | qayssarayra.com
