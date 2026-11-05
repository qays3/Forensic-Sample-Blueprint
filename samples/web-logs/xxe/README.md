# xxe

## Scenario
An attacker sends XML with an external entity (XXE) to an import endpoint to read local files or reach internal services.

## Timeline (UTC+3)
```
14:30:00  POST /api/v1/import with an XML external entity
14:30:xx  DOCTYPE with SYSTEM file:///etc/passwd
```

## How to spot it
- POST of XML to an import/parse endpoint
- A DOCTYPE with an external SYSTEM entity (in the body)
- Reads of local files via file:// 
- Possible 500 on malformed entity, 200 on success

## Questions
```
1. Endpoint?          /api/v1/import
2. Attack?            XXE (XML external entity)
3. Entity target?     file:///etc/passwd
4. Body type?         XML with DOCTYPE SYSTEM
5. Where is payload?  Request body (app log)
```

## How to solve it

1. Extract the evidence, password `hidden`: `7z x afbd6146642a061fba229161bb68d761533cfbb83959063278c14ad285433b96.zip -phidden` (or your archive tool). You get `access.log`.
2. Find the busiest source: `awk '{print $1}' access.log | sort | uniq -c | sort -rn | head`.
3. Pull the suspect requests and their status codes: `grep -Ei 'select|union|\.\./|<script|/admin|\.php|payload' access.log` (adjust to this attack), and `awk '{print $9}' access.log | sort | uniq -c`.
4. Confirm against the answer: POST of XML to an import/parse endpoint

## MITRE ATT&CK
- T1190 Exploit Public-Facing Application

## Tools
Hand written Apache access log with real workday background traffic

## Signature
Forensic Sample Blueprint | qayssarayra.com
