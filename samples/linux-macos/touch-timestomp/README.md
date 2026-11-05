# touch-timestomp

## Scenario
An attacker uses touch to backdate a malicious file so it blends with old system files.

## Timeline
```
~16:01  touch backdates /tmp/.x to look old
```

## How to spot it
- touch -t or -r changing a file timestamp
- A file whose mtime predates its real creation
- Timestamps matched to system binaries
- Anti forensics (timestomp)

## Questions
```
1. Technique?          Timestomping via touch
2. File?               /tmp/.x
3. Methods?            touch -t, touch -r
4. Goal?               Blend with old files
5. Host?               appsrv02
```

## How to solve it

1. Extract the evidence, password `hidden`: `7z x f7ccbdc7732e985948e8f4e36425c65fa749e063a70304214ef02586b4a25cdf.zip -phidden` (or your archive tool). You get `auth.log`.
2. Read the log: `less auth.log`; for auditd use `ausearch -if auth.log`, for a journal export `journalctl --file=auth.log`.
3. Grep the indicator: `grep -Ei 'failed|accepted|sudo|useradd|/tmp/|preload|cron|wget|curl' auth.log` (narrow to this attack).
4. Confirm against the answer: touch -t or -r changing a file timestamp

## MITRE ATT&CK
- T1070.006 Indicator Removal: Timestomp

## Tools
Hand written Linux logs with a workday of normal activity as background

## Signature
Forensic Sample Blueprint | qayssarayra.com
