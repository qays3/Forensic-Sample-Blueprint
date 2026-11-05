# Cloud Bucket Enumeration

## Scenario

10.10.1.66 guessed S3 bucket names under corp-* and probed them over HTTP, getting a mix of 200 (public, listable), 403 (exists, private) and 404 (no such bucket).

Evidence: `cloud-bucket-enumeration.pcap`  
Capture date: 2020-12-03

## Timeline

~14:01  Sequential GET / to <name>.s3.amazonaws.com with 200/403/404 answers mapping which buckets exist.

## How to spot it

- Many GET / to guessed bucket hostnames
- 200 means public and listable, 403 means exists but private, 404 means none
- A wordlist of corp- prefixed names
- Existence leaks from 403 vs 404

## Questions

```
1. Scanner?           10.10.1.66
2. Target?             S3 (bucket names)
3. 200?                corp-public, corp-assets, corp-images
4. 403 (exists)?        corp-backups, corp-finance, corp-hr
5. Technique?          Cloud bucket enumeration
```

## MITRE ATT&CK

- T1580 Cloud Infrastructure Discovery
- T1526 Cloud Service Discovery

## Tools

Wireshark, tshark, zeek, tcpdump. Filter by the scanner IP to isolate the activity.

## Signature

Forensic Sample Blueprint | qayssarayra.com
