# capabilities-abuse

## Scenario
An attacker finds a binary with cap_setuid and uses it to become root.

## Timeline
```
~13:10  getcap enumeration then python3 setuid(0) to root
```

## How to spot it
- getcap -r enumerating file capabilities
- A binary with cap_setuid abused
- python3 os.setuid(0) spawning a shell
- Root without sudo or SUID

## Questions
```
1. Enumeration?        getcap -r /
2. Capability?          cap_setuid
3. Binary?              python3
4. Result?              Root shell
5. Host?                appsrv02
```

## MITRE ATT&CK
- T1548 Abuse Elevation Control Mechanism

## Tools
Hand written Linux logs with a workday of normal activity as background

## Signature
Forensic Sample Blueprint | qayssarayra.com
