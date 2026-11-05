# Imds Credential Theft

## Scenario

Instance metadata credential theft from a compromised server: an HTTP GET to the link local metadata address 169.254.169.254 pulls the instance role credentials.

Evidence: `imds-credential-theft.pcap`  
Capture date: 2022-12-27

## Timeline

~10:15  HTTP GET to 169.254.169.254 for the IAM security credentials.

## How to spot it

- HTTP request to 169.254.169.254
- Path under /latest/meta-data/iam/security-credentials
- Temporary keys returned in JSON
- Often from a server side request or a shell on the host

## Questions

```
1. Target?             169.254.169.254 (IMDS)
2. Path?               iam/security-credentials
3. Stolen?             instance role keys
4. Source?             compromised host
5. ATT&CK?             T1552.005
```

## How to solve it

1. Extract the evidence, password `hidden`: `7z x <sha256>.zip -phidden`. You get `imds-credential-theft.pcap`.
2. Open `imds-credential-theft.pcap` in Wireshark.
3. In Wireshark, paste this into the display filter bar and press Enter: `ip.addr==169.254.169.254`  (the metadata request). Then Statistics > Conversations for the talkers.
4. Same filter from the shell: `tshark -r imds-credential-theft.pcap -Y "ip.addr==169.254.169.254"`, and `tshark -r imds-credential-theft.pcap -q -z conv,ip`.
5. Confirm against the answer: HTTP request to 169.254.169.254

## MITRE ATT&CK

- T1552.005 Cloud Instance Metadata API

## Tools

Wireshark, tshark, zeek, tcpdump.

## Signature

Forensic Sample Blueprint | qayssarayra.com
