# bind-shell

## Scenario
An attacker opens a bind shell, listening on a port so they can connect in and get a shell.

## Timeline
```
~14:30  A bind shell listener opens on port 9001
```

## How to spot it
- A new listener on an odd high port
- nc with -e /bin/bash
- Inbound connections to that port follow
- Bind (not reverse) shell

## Questions
```
1. Type?               Bind shell
2. Port?               9001
3. Tool?               nc -e /bin/bash
4. Direction?          Attacker connects in
5. Host?               appsrv02
```

## MITRE ATT&CK
- T1059.004 Unix Shell

## Tools
Hand written Linux logs with a workday of normal activity as background

## Signature
Forensic Sample Blueprint | qayssarayra.com
