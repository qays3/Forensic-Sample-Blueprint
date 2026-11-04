# ssh-lateral-movement

## Scenario
An attacker uses a stolen key to SSH from one internal host to others, moving laterally.

## Timeline
```
~15:00  SSH from an internal host to 10.10.2.30/31 using a stolen key
```

## How to spot it
- Internal to internal SSH with a key
- Logins originating from an already compromised host
- Hopping across servers
- Stolen key reuse

## Questions
```
1. Technique?          SSH lateral movement
2. Auth?               Stolen private key
3. From?               10.10.2.22
4. To?                 10.10.2.30, 10.10.2.31
5. Host logged?        jumphost
```

## MITRE ATT&CK
- T1021.004 Remote Services: SSH
- T1570 Lateral Tool Transfer

## Tools
Hand written Linux logs with a workday of normal activity as background

## Signature
Forensic Sample Blueprint | qayssarayra.com
