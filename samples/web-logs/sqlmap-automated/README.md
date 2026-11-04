# sqlmap-automated

## Scenario
The attacker runs sqlmap against /products. The log shows hundreds of automated injection probes with the sqlmap user agent and encoded payloads.

## Timeline (UTC+3)
```
14:00:00  sqlmap starts hammering /products?id= from 45.137.22.90
14:0x:xx  Hundreds of encoded SQLi probes in a short time
```

## How to spot it
- Hundreds of requests to one parameter in seconds
- URL encoded SQL payloads and ORDER BY enumeration
- sqlmap user agent (or a generic one at high rate)
- Mix of 200 and 500 as it maps the injection

## Questions
```
1. Target param?      id on /products
2. Tool?              sqlmap
3. Clue?              High rate, encoded payloads, ORDER BY probing
4. Volume?            Hundreds of requests
5. Status mix?        200 and 500
```

## MITRE ATT&CK
- T1190 Exploit Public-Facing Application

## Tools
Hand written Apache access log with real workday background traffic (set the sqlmap UA if you want it explicit)

## Signature
Forensic Sample Blueprint | qayssarayra.com
