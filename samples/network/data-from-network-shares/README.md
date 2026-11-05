# Data From Network Shares

## Scenario

Bulk reading of files from a network share by 10.10.1.66 on 10.10.2.61: many documents are opened and read from a Finance share in a short window.

Evidence: `data-from-network-shares.pcap`  
Capture date: 2022-06-01

## Timeline

~10:00  Many SMB2 reads of documents from a Finance share from one host.

## How to spot it

- Many SMB2 reads from a share
- One host pulling many files quickly
- Office documents read in bulk
- Collection from a network share

## Questions

```
1. Source?             10.10.1.66
2. Share?              Finance
3. Pattern?            many reads, short window
4. Data?               bulk documents
5. ATT&CK?             T1039
```

## How to solve it

1. Extract the evidence, password `hidden`: `7z x <sha256>.zip -phidden`. You get `data-from-network-shares.pcap`.
2. Open `data-from-network-shares.pcap` in Wireshark.
3. In Wireshark, paste this into the display filter bar and press Enter: `tcp.port==445`  (the bulk reads). Then Statistics > Conversations for the talkers.
4. Same filter from the shell: `tshark -r data-from-network-shares.pcap -Y "tcp.port==445"`, and `tshark -r data-from-network-shares.pcap -q -z conv,ip`.
5. Confirm against the answer: Many SMB2 reads from a share

## MITRE ATT&CK

- T1039 Data from Network Shared Drive

## Tools

Wireshark, tshark, zeek, tcpdump.

## Signature

Forensic Sample Blueprint | qayssarayra.com
