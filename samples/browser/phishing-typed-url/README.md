# Phishing Typed Url

## Scenario

The urls table shows the user typed a Microsoft login lookalike domain, the hallmark of a credential phishing visit.

Host: WS-FIN-07  
Evidence: `History` (Chrome/Edge History SQLite)  
Activity date: 2019-09-10

## Timeline

~08:45  Typed a login.microsoftonline.com lookalike (typed_count 1) and submitted to secure-corp-sso.com.

## How to spot it

- A typed_count of 1 on a lookalike login domain
- Domain embeds microsoftonline.com as a subdomain trick
- Visited directly, not via a link
- Followed immediately by the auth page

## Questions

```
1. Table?             urls (typed_count)
2. Domain?             ...secure-corp-sso.com
3. Mimics?             login.microsoftonline.com
4. typed_count?         1 (typed by hand)
5. Technique?          Credential phishing
```

## MITRE ATT&CK

- T1566.002 Spearphishing Link
- T1056 Input Capture

## Tools

DB Browser for SQLite, Hindsight, or `sqlite3`. Chrome timestamps are microseconds since 1601-01-01 UTC.

## Signature

Forensic Sample Blueprint | qayssarayra.com
