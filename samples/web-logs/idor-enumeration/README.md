# idor-enumeration

## Scenario
A logged in user walks sequential invoice IDs to read other users' documents (insecure direct object reference).

## Timeline (UTC+3)
```
12:10:00  10.10.1.45 requests /api/v1/invoices/1000, 1001, 1002 ...
12:10:xx  Sequential IDs, some 200 (other users' data)
```

## How to spot it
- One user requesting many sequential object IDs
- Some return 200 for records that are not theirs
- No access control on the object reference
- Steady increment pattern

## Questions
```
1. Attack?            IDOR
2. Endpoint?          /api/v1/invoices/{id}
3. Pattern?           Sequential ID walk
4. Leak sign?         200 on others' invoices
5. User?              10.10.1.45
```

## MITRE ATT&CK
- T1190 Exploit Public-Facing Application

## Tools
Hand written Apache access log with real workday background traffic

## Signature
Forensic Sample Blueprint | qayssarayra.com
