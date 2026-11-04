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

## MITRE ATT&CK
- T1070.006 Indicator Removal: Timestomp

## Tools
Hand written Linux logs with a workday of normal activity as background

## Signature
Forensic Sample Blueprint | qayssarayra.com
