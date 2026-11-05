# Smb Admin Share Copy

## Scenario

A payload is copied to the C$ admin share from 10.10.1.66 to 10.10.2.18: an SMB write of an executable into Windows\Temp over the admin share.

Evidence: `smb-admin-share-copy.pcap`  
Capture date: 2019-07-15

## Timeline

~13:05  SMB session to C$, an SMB2 create and write of payload.exe into Windows\Temp.

## How to spot it

- SMB write to the C$ admin share
- An executable written to a system folder
- Large SMB2 write operation
- From the operator host

## Questions

```
1. Share?             C$
2. File?               Windows\Temp\payload.exe
3. Operation?           SMB2 write
4. Source?             operator host
5. Technique?          Admin share copy
```

## How to solve it

1. Extract the evidence, password `hidden`: `7z x <sha256>.zip -phidden`. You get `smb-admin-share-copy.pcap`.
2. Open `smb-admin-share-copy.pcap` in Wireshark.
3. In Wireshark, paste this into the display filter bar and press Enter: `tcp.port==445`  (the C$ write). Then Statistics > Conversations for the talkers.
4. Same filter from the shell: `tshark -r smb-admin-share-copy.pcap -Y "tcp.port==445"`, and `tshark -r smb-admin-share-copy.pcap -q -z conv,ip`.
5. Confirm against the answer: SMB write to the C$ admin share

## MITRE ATT&CK

- T1021.002 SMB/Admin Shares
- T1570 Lateral Tool Transfer

## Tools

Wireshark, tshark, zeek, tcpdump.

## Signature

Forensic Sample Blueprint | qayssarayra.com
