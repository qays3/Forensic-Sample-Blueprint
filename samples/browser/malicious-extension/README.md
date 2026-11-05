# Malicious Extension

## Scenario

The browser Secure Preferences show a sideloaded extension (not from the Web Store) with an external update_url, the registry of a force installed malicious extension.

Host: WS-FIN-07  
Evidence: `Secure Preferences` (Chrome Secure Preferences JSON)  
Activity date: 2019-06-15

## Timeline

An extension Corp Helper with from_webstore false and an update_url to an unknown CDN, alongside normal store extensions.

## How to spot it

- An extension with from_webstore = false
- An external update_url (not Google)
- Sideloaded, not user installed from the store
- Sits among legit default extensions

## Questions

```
1. Artifact?          Secure Preferences
2. Extension?          Corp Helper
3. Tell?               from_webstore false + external update_url
4. Normal ones?         from_webstore true
5. Technique?          Malicious browser extension
```

## MITRE ATT&CK

- T1176 Browser Extensions

## Tools

DB Browser for SQLite, Hindsight, or `sqlite3`. Chrome timestamps are microseconds since 1601-01-01 UTC.

## Signature

Forensic Sample Blueprint | qayssarayra.com
