# Fileless Registry Payload

## Scenario

A PE payload is stored as a large binary registry value and a Run key loads it with PowerShell, keeping the payload off disk.

Host: WS-FIN-07  
Evidence: `registry.reg` (exported registry keys)  
Approx hive last write: 2021-09-26 12:30

## Timeline

A big binary value under a Classes key (starts with MZ) and a Run value that reads and executes it.

## How to spot it

- An unusually large binary value starting with 4d,5a (MZ)
- A Run value that reads that value with PowerShell
- Payload lives in the registry, not a file
- iex / GetString pulling the blob at logon

## Questions

```
1. Where is payload?  A binary registry value (MZ header)
2. Loader?             Run key PowerShell iex
3. On disk?            No, fileless
4. Fires at?           Logon
5. Technique?          Fileless registry payload
```

## MITRE ATT&CK

- T1547.001 Registry Run Keys
- T1027 Obfuscated/Fileless

## Tools

Registry Explorer, regedit, RECmd, reg query. Open the .reg in a text editor to read the keys directly.

## Signature

Forensic Sample Blueprint | qayssarayra.com
