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

## MITRE ATT&CK
- T1190 Exploit Public-Facing Application

## Tools
Hand written Apache access log with real workday background traffic

## Signature
Forensic Sample Blueprint | qayssarayra.com
