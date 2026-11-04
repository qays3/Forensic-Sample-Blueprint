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

## MITRE ATT&CK
- T1560.001 Archive Collected Data
- T1048 Exfiltration Over Alternative Protocol

## Tools
Hand written Linux logs with a workday of normal activity as background

## Signature
Forensic Sample Blueprint | qayssarayra.com
