# history-wipe

## Scenario
After the attack the intruder clears shell history to cover their tracks.

## Timeline
```
~16:00  HISTFILE unset, history -c, .bash_history removed
```

## How to spot it
- HISTFILE set to /dev/null or unset
- history -c and removal of .bash_history
- A suspiciously empty or truncated history
- Anti forensics after activity

## Questions
```
1. Technique?          Shell history wipe
2. Commands?           unset HISTFILE, history -c, rm history
3. Goal?               Cover tracks
4. Tell?               Empty/short history
5. Host?               websrv01
```

## How to solve it

1. Extract the evidence, password `hidden`: `7z x 134044cb44c0963b41287b3a97d72200f5f98a3ba9056b4bdbe6d8994d90a9f0.zip -phidden` (or your archive tool). You get `auth.log`.
2. Read the log: `less auth.log`; for auditd use `ausearch -if auth.log`, for a journal export `journalctl --file=auth.log`.
3. Grep the indicator: `grep -Ei 'failed|accepted|sudo|useradd|/tmp/|preload|cron|wget|curl' auth.log` (narrow to this attack).
4. Confirm against the answer: HISTFILE set to /dev/null or unset

## MITRE ATT&CK
- T1070.003 Indicator Removal: Clear Command History

## Tools
Hand written Linux logs with a workday of normal activity as background

## Signature
Forensic Sample Blueprint | qayssarayra.com
