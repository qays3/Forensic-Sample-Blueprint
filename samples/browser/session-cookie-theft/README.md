# Session Cookie Theft

## Scenario

A copy of the Chrome Cookies database holds live session cookies for Google, Microsoft, Okta, GitHub, and AWS, the prize in a cookie theft that enables session hijacking without passwords.

Host: WS-FIN-07  
Evidence: `Cookies` (Chrome Cookies SQLite)  
Activity date: 2022-10-17

## Timeline

Session cookies (SID, ESTSAUTH, Okta sid, GitHub user_session, AWS) present with future expiry and recent last access.

## How to spot it

- Auth/session cookie names for SSO and cloud
- encrypted_value present (decryptable on the host)
- Cookies not yet expired, recently accessed
- Copying this file enables session replay

## Questions

```
1. Artifact?          Cookies SQLite
2. Targets?            Google, MS, Okta, GitHub, AWS
3. Value?               Live session tokens
4. Enables?             Session hijack, no password
5. Technique?          Steal web session cookie
```

## MITRE ATT&CK

- T1539 Steal Web Session Cookie

## Tools

DB Browser for SQLite, Hindsight, or `sqlite3`. Chrome timestamps are microseconds since 1601-01-01 UTC.

## Signature

Forensic Sample Blueprint | qayssarayra.com
