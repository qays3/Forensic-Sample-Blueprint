# binary-in-tmp-devshm

## Scenario
An attacker runs a binary from /dev/shm (memory backed, often missed) to evade disk based checks.

## Timeline
```
~14:31  Execution of /dev/shm/.k from memory backed storage
```

## How to spot it
- Execution from /tmp or /dev/shm
- Hidden (dot) binary names
- Memory backed path to dodge disk scans
- Odd process paths

## Questions
```
1. Path?               /dev/shm/.k
2. Why there?          Memory backed, often not scanned
3. Hidden?             Dot prefix name
4. Technique?          Run from tmp/shm
5. Host?               appsrv02
```

## MITRE ATT&CK
- T1620 Reflective Code Loading
- T1036 Masquerading

## Tools
Hand written Linux logs with a workday of normal activity as background

## Signature
Forensic Sample Blueprint | qayssarayra.com
