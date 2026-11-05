# Remote Share Staging

## Scenario

Collected data is staged on a file server from 10.10.1.66 to 10.10.2.60: a large archive is written to a staging folder over SMB before exfiltration.

Evidence: `remote-share-staging.pcap`  
Capture date: 2020-04-19

## Timeline

~10:01  A large archive written to a staging folder on the file server over SMB.

## How to spot it

- A large archive written to a share
- Staging folder path
- Big SMB2 write operations
- Data gathered before exfil

## Questions

```
1. Stage?             collection staging
2. Target?             file server share
3. File?               collected.7z
4. Operation?           SMB2 write
5. ATT&CK?             T1074.001
```

## How to solve it

1. Extract the evidence, password `hidden`: `7z x <sha256>.zip -phidden`. You get `remote-share-staging.pcap`.
2. Open `remote-share-staging.pcap` in Wireshark.
3. In Wireshark, paste this into the display filter bar and press Enter: `tcp.port==445`  (the archive write). Then Statistics > Conversations for the talkers.
4. Same filter from the shell: `tshark -r remote-share-staging.pcap -Y "tcp.port==445"`, and `tshark -r remote-share-staging.pcap -q -z conv,ip`.
5. Confirm against the answer: A large archive written to a share

## MITRE ATT&CK

- T1074.001 Local Data Staging
- T1039 Data from Network Shared Drive

## Tools

Wireshark, tshark, zeek, tcpdump.

## Signature

Forensic Sample Blueprint | qayssarayra.com
