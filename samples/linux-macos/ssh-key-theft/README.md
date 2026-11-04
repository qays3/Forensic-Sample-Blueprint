# ssh-key-theft

## Scenario
An attacker hunts for and steals SSH private keys to move to other hosts.

## Timeline
```
~14:37  Private keys found and copied from user home directories
```

## How to spot it
- find for id_rsa across home directories
- Reads and copies of private keys
- Keys staged in /tmp
- Enables lateral movement

## Questions
```
1. Target?             SSH private keys (id_rsa)
2. Where?              user .ssh folders
3. Action?             Copy to /tmp
4. Use?                Lateral movement
5. Host?               jumphost
```

## MITRE ATT&CK
- T1552.004 Unsecured Credentials: Private Keys

## Tools
Hand written Linux logs with a workday of normal activity as background

## Signature
Forensic Sample Blueprint | qayssarayra.com
