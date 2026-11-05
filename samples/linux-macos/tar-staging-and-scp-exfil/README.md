# tar-staging-and-scp-exfil

## Scenario
An attacker archives a file share and scp's it out, then deletes the archive.

## Timeline
```
~15:07  tar of /srv/share, then scp to 45.137.22.90, then cleanup
```

## How to spot it
- A tar/gzip of a data directory into /tmp
- scp or sftp of the archive to an external host
- Cleanup (rm) of the archive after
- Stage then exfil pattern

## Questions
```
1. Staged?             tar of /srv/share -> /tmp/.data.tgz
2. Exfil?              scp to 45.137.22.90
3. Cleanup?            rm the archive
4. Phase?              Collection + exfil
5. Host?               fileserv
```

## How to solve it

1. Extract the evidence, password `hidden`: `7z x a0df5ad6a08804adaab5ce4e85a2c2c25a8037c35c26b6a25942da940af3a1ac.zip -phidden` (or your archive tool). You get `auth.log`.
2. Read the log: `less auth.log`; for auditd use `ausearch -if auth.log`, for a journal export `journalctl --file=auth.log`.
3. Grep the indicator: `grep -Ei 'failed|accepted|sudo|useradd|/tmp/|preload|cron|wget|curl' auth.log` (narrow to this attack).
4. Confirm against the answer: A tar/gzip of a data directory into /tmp

## MITRE ATT&CK
- T1560.001 Archive Collected Data
- T1048 Exfiltration Over Alternative Protocol

## Tools
Hand written Linux logs with a workday of normal activity as background

## Signature
Forensic Sample Blueprint | qayssarayra.com
