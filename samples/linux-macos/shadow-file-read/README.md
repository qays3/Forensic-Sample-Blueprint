# shadow-file-read

## Scenario
An attacker reads /etc/shadow and prepares it for offline password cracking.

## Timeline
```
~14:35  /etc/shadow read and combined with passwd for cracking
```

## How to spot it
- A non root read of /etc/shadow
- Copy of shadow to a temp location
- unshadow combining passwd and shadow
- Prep for offline cracking

## Questions
```
1. File?               /etc/shadow
2. Goal?               Offline password cracking
3. Tool?               unshadow
4. Output?             /tmp/crack.txt
5. Host?               appsrv02
```

## MITRE ATT&CK
- T1003.008 OS Credential Dumping: /etc/passwd and /etc/shadow

## Tools
Hand written Linux logs with a workday of normal activity as background

## Signature
Forensic Sample Blueprint | qayssarayra.com
