# linux-recon-commands

## Scenario
After a foothold, the attacker runs standard Linux host and network enumeration.

## Timeline
```
~10:47  Burst of recon commands as www-data
```

## How to spot it
- A burst of enumeration commands together
- id, uname, ps, ss, cat /etc/passwd, sudo -l
- Host and network discovery
- Right after initial access

## Questions
```
1. Phase?              Discovery
2. Commands?           id, uname, ps, ss, sudo -l ...
3. User?               www-data
4. Evidence?           bash_history
5. Host?               websrv01
```

## MITRE ATT&CK
- T1082 System Information Discovery
- T1033 System Owner/User Discovery

## Tools
Hand written Linux logs with a workday of normal activity as background

## Signature
Forensic Sample Blueprint | qayssarayra.com
