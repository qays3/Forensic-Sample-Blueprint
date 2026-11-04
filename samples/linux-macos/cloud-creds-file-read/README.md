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

## MITRE ATT&CK
- T1552.001 Unsecured Credentials: Credentials In Files

## Tools
Hand written Linux logs with a workday of normal activity as background

## Signature
Forensic Sample Blueprint | qayssarayra.com
