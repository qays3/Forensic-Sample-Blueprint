# sudo-gtfobins-abuse

## Scenario
A user with a narrow sudo right abuses find via GTFOBins to spawn a root shell, then reads /etc/shadow.

## Timeline
```
~13:00  sudo find -exec /bin/sh used by ahmad to get root
```

## How to spot it
- A sudo command running find with -exec /bin/sh
- A normal user suddenly acting as root
- GTFOBins style escape from an allowed binary
- Root shell then sensitive file reads

## Questions
```
1. User?               ahmad
2. Binary abused?      find (via sudo)
3. Technique?          GTFOBins -exec /bin/sh
4. After root?         cat /etc/shadow
5. Host?               appsrv02
```

## MITRE ATT&CK
- T1548.003 Abuse Elevation Control Mechanism: Sudo and Sudo Caching

## Tools
Hand written Linux logs with a workday of normal activity as background

## Signature
Forensic Sample Blueprint | qayssarayra.com
