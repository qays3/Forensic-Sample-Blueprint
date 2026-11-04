# authorized-keys-persistence

## Scenario
An attacker adds their SSH key to root's authorized_keys for passwordless persistent access.

## Timeline
```
~14:25  Attacker key appended to /root/.ssh/authorized_keys, then publickey login
```

## How to spot it
- A new line in an authorized_keys file
- Later publickey logins for that account from outside
- No password prompt, key based
- Persistent backdoor access

## Questions
```
1. Technique?          authorized_keys persistence
2. Account?            root
3. File?               /root/.ssh/authorized_keys
4. Login type?         publickey
5. Source?             45.137.22.90
```

## MITRE ATT&CK
- T1098.004 Account Manipulation: SSH Authorized Keys

## Tools
Hand written Linux logs with a workday of normal activity as background

## Signature
Forensic Sample Blueprint | qayssarayra.com
