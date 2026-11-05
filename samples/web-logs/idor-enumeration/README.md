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

## How to solve it

1. Extract the evidence, password `hidden`: `7z x 52c3d1bb0a1d21079260d6bce119b20b7507386200a74c83df4c43c7f191cbe6.zip -phidden` (or your archive tool). You get `access.log`.
2. Find the busiest source: `awk '{print $1}' access.log | sort | uniq -c | sort -rn | head`.
3. Pull the suspect requests and their status codes: `grep -Ei 'select|union|\.\./|<script|/admin|\.php|payload' access.log` (adjust to this attack), and `awk '{print $9}' access.log | sort | uniq -c`.
4. Confirm against the answer: One user requesting many sequential object IDs

## MITRE ATT&CK
- T1190 Exploit Public-Facing Application

## Tools
Hand written Apache access log with real workday background traffic

## Signature
Forensic Sample Blueprint | qayssarayra.com
