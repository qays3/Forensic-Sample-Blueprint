# bashrc-persistence

## Scenario
An attacker appends a reverse shell line to .bashrc so it runs on every login.

## Timeline
```
~14:23  Reverse shell line added to ~/.bashrc
```

## How to spot it
- A reverse shell appended to .bashrc or .profile
- Runs on each interactive login
- Outbound to an attacker IP on login
- Shell init file modified

## Questions
```
1. File?               ~/.bashrc
2. Payload?            bash reverse shell to 45.137.22.90:4444
3. Trigger?            Every login
4. Technique?          Shell init persistence
5. User?               www-data
```

## MITRE ATT&CK
- T1546.004 Event Triggered Execution: Unix Shell Configuration Modification

## Tools
Hand written Linux logs with a workday of normal activity as background

## Signature
Forensic Sample Blueprint | qayssarayra.com
