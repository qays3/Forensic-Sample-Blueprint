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

## How to solve it

1. Extract the evidence, password `hidden`: `7z x 759972b3b9b6e9dcbd2cfc4b4119ce38f632a1549d81a4e7f13034d08a8a5f90.zip -phidden` (or your archive tool). You get `auth.log`.
2. Read the log: `less auth.log`; for auditd use `ausearch -if auth.log`, for a journal export `journalctl --file=auth.log`.
3. Grep the indicator: `grep -Ei 'failed|accepted|sudo|useradd|/tmp/|preload|cron|wget|curl' auth.log` (narrow to this attack).
4. Confirm against the answer: Execution from /tmp or /dev/shm

## MITRE ATT&CK
- T1620 Reflective Code Loading
- T1036 Masquerading

## Tools
Hand written Linux logs with a workday of normal activity as background

## Signature
Forensic Sample Blueprint | qayssarayra.com
