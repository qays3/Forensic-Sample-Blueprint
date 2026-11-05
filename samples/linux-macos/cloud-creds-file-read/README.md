# cloud-creds-file-read

## Scenario
An attacker reads cloud credential files (.aws/credentials) to pivot into the cloud.

## Timeline
```
~14:40  AWS and GCP credential files read
```

## How to spot it
- Reads of ~/.aws/credentials or gcloud configs
- env scraped for AWS keys
- Cloud secrets on a Linux host
- Cloud pivot prep

## Questions
```
1. Files?              ~/.aws/credentials, gcloud creds
2. Goal?               Pivot to cloud
3. Also checked?       env for AWS keys
4. Technique?          Credentials in files
5. Host?               appsrv02
```

## How to solve it

1. Extract the evidence, password `hidden`: `7z x d79ffbcb6ee7631c52d8fbf11b9f20cc283d96192fc428bb487fcbbafd70cdbf.zip -phidden` (or your archive tool). You get `auth.log`.
2. Read the log: `less auth.log`; for auditd use `ausearch -if auth.log`, for a journal export `journalctl --file=auth.log`.
3. Grep the indicator: `grep -Ei 'failed|accepted|sudo|useradd|/tmp/|preload|cron|wget|curl' auth.log` (narrow to this attack).
4. Confirm against the answer: Reads of ~/.aws/credentials or gcloud configs

## MITRE ATT&CK
- T1552.001 Unsecured Credentials: Credentials In Files

## Tools
Hand written Linux logs with a workday of normal activity as background

## Signature
Forensic Sample Blueprint | qayssarayra.com
