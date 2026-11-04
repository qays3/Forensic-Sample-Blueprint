# pam-backdoor

## Scenario
An attacker replaces a PAM module with a backdoored one that accepts a magic password for any account.

## Timeline
```
~14:26  Backdoored pam_unix.so installed, root login accepted with magic password
```

## How to spot it
- A replaced PAM module (pam_unix.so)
- Logins accepted that should not be
- A magic password working for any user
- Hard to spot without file integrity checks

## Questions
```
1. Module?             pam_unix.so
2. Technique?          PAM backdoor
3. Effect?             Magic password for any account
4. Evidence?           Modified module, odd root login
5. Host?               jumphost
```

## MITRE ATT&CK
- T1556.003 Modify Authentication Process: Pluggable Authentication Modules

## Tools
Hand written Linux logs with a workday of normal activity as background

## Signature
Forensic Sample Blueprint | qayssarayra.com
