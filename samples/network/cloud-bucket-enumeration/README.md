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

## How to solve it

1. Extract the evidence, password `hidden`: `7z x 4efb2669946962e3919cfa15e566b49562461ef4fe07d4fd84d9efca6375ed1e.zip -phidden`. You get `cloud-bucket-enumeration.pcap`.
2. Open `cloud-bucket-enumeration.pcap` in Wireshark.
3. In Wireshark, paste this into the display filter bar and press Enter: `http || tls`  (HTTP/TLS requests, hosts and user agents). Then Statistics > Conversations to see the talkers.
4. Same filter from the shell with tshark: `tshark -r cloud-bucket-enumeration.pcap -Y "http || tls"`, and list talkers with `tshark -r cloud-bucket-enumeration.pcap -q -z conv,ip`.
5. Confirm against the answer: Many GET / to guessed bucket hostnames

## MITRE ATT&CK

- T1580 Cloud Infrastructure Discovery
- T1526 Cloud Service Discovery

## Tools

Wireshark, tshark, zeek, tcpdump. Filter by the scanner IP to isolate the activity.

## Signature

Forensic Sample Blueprint | qayssarayra.com
