# new-local-user

## Scenario
An attacker creates a hidden UID 0 account (svc-sync) for persistent root access.

## Timeline
```
~14:28  useradd creates svc-sync with UID 0 (root equivalent)
```

## How to spot it
- A new account created with UID 0
- useradd -o -u 0 (duplicate root)
- A service looking name to blend in
- Backdoor root account

## Questions
```
1. Account?            svc-sync
2. UID?                0 (root)
3. Flag?               useradd -o -u 0
4. Why?                Persistent root
5. Host?               appsrv02
```

## MITRE ATT&CK
- T1136.001 Create Account: Local Account

## Tools
Hand written Linux logs with a workday of normal activity as background

## Signature
Forensic Sample Blueprint | qayssarayra.com
