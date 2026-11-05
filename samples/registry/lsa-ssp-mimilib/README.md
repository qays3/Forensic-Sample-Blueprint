# Lsa Ssp Mimilib

## Scenario

An extra Security Package (mimilib) is added to LSA, so a malicious SSP loads into lsass and can capture credentials.

Host: WS-FIN-07  
Evidence: `registry.reg` (exported registry keys)  
Approx hive last write: 2021-09-06 09:01

## Timeline

The LSA Security Packages multi string now includes mimilib alongside the normal packages.

## How to spot it

- An unexpected name in LSA Security Packages
- Loads into lsass as an SSP
- Can log plaintext credentials
- Normal list is well known and short

## Questions

```
1. Key?               Lsa\Security Packages
2. Added?              mimilib
3. Loads into?          lsass
4. Capability?          Capture creds
5. Technique?          Malicious SSP
```

## MITRE ATT&CK

- T1547.005 Security Support Provider

## Tools

Registry Explorer, regedit, RECmd, reg query. Open the .reg in a text editor to read the keys directly.

## Signature

Forensic Sample Blueprint | qayssarayra.com
