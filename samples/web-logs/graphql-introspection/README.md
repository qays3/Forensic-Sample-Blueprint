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

## MITRE ATT&CK
- T1595 Active Scanning

## Tools
Hand written Apache access log with real workday background traffic

## Signature
Forensic Sample Blueprint | qayssarayra.com
