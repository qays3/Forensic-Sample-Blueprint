# domain-fronting

## Scenario
A host (10.10.1.59) uses domain fronting. The TLS SNI says ajax.googleapis.com (a trusted CDN) but the inner HTTP Host header points to evil-c2-backend.com. The CDN routes the request to the hidden backend, so the traffic looks like it goes to Google.

## Timeline (UTC+3)
```
09:28:20  Fronted request: SNI ajax.googleapis.com, Host evil-c2-backend.com
all day   Repeats about every 90s, ~222 times
```

## How to spot it
- The TLS SNI and the HTTP Host header do not match
- Outer destination is a trusted CDN, inner Host is the real C2
- Looks like normal CDN traffic at the network edge
- Requires reading the inner Host to catch it

## Questions
```
1. Host?             10.10.1.59
2. SNI (outer)?      ajax.googleapis.com
3. Host header (inner)?  evil-c2-backend.com
4. Technique?         Domain fronting
5. Why it hides?     Traffic appears to go to the CDN
```

## MITRE ATT&CK
- T1090.004 Proxy: Domain Fronting
- T1071.001 Web Protocols

## Tools
Crafted domain fronting TLS/HTTP (scapy) over generated background traffic

## Signature
Forensic Sample Blueprint | qayssarayra.com
