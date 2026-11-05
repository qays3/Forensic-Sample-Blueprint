# graphql-introspection

## Scenario
An attacker runs GraphQL introspection and query batching against /graphql to map the whole schema and pull data.

## Timeline (UTC+3)
```
14:45:00  POST /graphql __schema introspection from 45.137.22.90
14:45:xx  Large responses, then batched queries
```

## How to spot it
- POSTs to /graphql with large responses
- Introspection (__schema) to dump the API shape
- Query batching to pull lots of data in one call
- Often left on by mistake in production

## Questions
```
1. Endpoint?          /graphql
2. Attack?            Introspection + batching
3. Introspection?     __schema query
4. Sign?              Very large responses
5. Risk?              Full schema and data exposure
```

## How to solve it

1. Extract the evidence, password `hidden`: `7z x 2a095047a74649660ac3e7be32ab41e74b677493f932ac0be6ecb202ea513e36.zip -phidden` (or your archive tool). You get `access.log`.
2. Find the busiest source: `awk '{print $1}' access.log | sort | uniq -c | sort -rn | head`.
3. Pull the suspect requests and their status codes: `grep -Ei 'select|union|\.\./|<script|/admin|\.php|payload' access.log` (adjust to this attack), and `awk '{print $9}' access.log | sort | uniq -c`.
4. Confirm against the answer: POSTs to /graphql with large responses

## MITRE ATT&CK
- T1595 Active Scanning

## Tools
Hand written Apache access log with real workday background traffic

## Signature
Forensic Sample Blueprint | qayssarayra.com
