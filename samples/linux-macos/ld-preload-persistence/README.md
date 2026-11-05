# ld-preload-persistence

## Scenario
An attacker adds a malicious shared object to /etc/ld.so.preload so it loads into every process.

## Timeline
```
~14:25  /etc/ld.so.preload points to /lib/.hook.so
```

## How to spot it
- A new entry in /etc/ld.so.preload
- A hidden .so loaded system wide
- Injected into every dynamically linked process
- Userland rootkit style persistence

## Questions
```
1. File?               /etc/ld.so.preload
2. Library?            /lib/.hook.so
3. Effect?             Loaded into all processes
4. Technique?          LD_PRELOAD rootkit
5. Host?               dbsrv01
```

## How to solve it

1. Extract the evidence, password `hidden`: `7z x 09e2d882c60b0f48baa06670e18ca24db9d590d3dd7be1a3b7f47c89491c58e2.zip -phidden` (or your archive tool). You get `auth.log`.
2. Read the log: `less auth.log`; for auditd use `ausearch -if auth.log`, for a journal export `journalctl --file=auth.log`.
3. Grep the indicator: `grep -Ei 'failed|accepted|sudo|useradd|/tmp/|preload|cron|wget|curl' auth.log` (narrow to this attack).
4. Confirm against the answer: A new entry in /etc/ld.so.preload

## MITRE ATT&CK
- T1574.006 Hijack Execution Flow: Dynamic Linker Hijacking

## Tools
Hand written Linux logs with a workday of normal activity as background

## Signature
Forensic Sample Blueprint | qayssarayra.com
