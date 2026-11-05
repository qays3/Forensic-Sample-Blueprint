# Winscp Sftp Exfil

## Scenario

WinSCP uploads data over SFTP to an external server: an SSH session on 22 to an unknown host moves files out.

Evidence: `winscp-sftp-exfil.pcap`  
Capture date: 2019-12-13

## Timeline

~14:05  An SSH/SFTP session with a WinSCP banner uploading a large volume to an external host.

## How to spot it

- SSH on 22 to an external, unknown host
- WinSCP client banner
- Large upload over SFTP
- Outbound file transfer

## Questions

```
1. Tool?              WinSCP (SFTP)
2. Port?               22
3. Destination?         203.0.113.77
4. Direction?           upload
5. Technique?          Exfil over SFTP
```

## How to solve it

1. Extract the evidence, password `hidden`: `7z x <sha256>.zip -phidden`. You get `winscp-sftp-exfil.pcap`.
2. Open `winscp-sftp-exfil.pcap` in Wireshark.
3. In Wireshark, paste this into the display filter bar and press Enter: `tcp.port==22 && ip.addr==203.0.113.77`  (The SFTP upload). Then Statistics > Conversations for the talkers.
4. Same filter from the shell: `tshark -r winscp-sftp-exfil.pcap -Y "tcp.port==22 && ip.addr==203.0.113.77"`, and `tshark -r winscp-sftp-exfil.pcap -q -z conv,ip`.
5. Confirm against the answer: SSH on 22 to an external, unknown host

## MITRE ATT&CK

- T1048 Exfiltration Over Alternative Protocol

## Tools

Wireshark, tshark, zeek, tcpdump.

## Signature

Forensic Sample Blueprint | qayssarayra.com
