# Taint Shared Content

## Scenario

Shared content is tainted: a malicious .lnk is written to a team share on 10.10.2.60 pointing at an attacker UNC path, so a victim browsing the folder auto authenticates to the attacker 10.10.1.66.

Evidence: `taint-shared-content.pcap`  
Capture date: 2020-04-20

## Timeline

~10:01  A .lnk written to a team share, then a victim auto connecting and authenticating to the attacker host.

## How to spot it

- A .lnk or similar written to a shared folder
- The shortcut points at an attacker UNC path
- A victim host later authenticates to the attacker
- SMB NTLM auth triggered by browsing the share

## Questions

```
1. Technique?         Taint shared content
2. Share?              FILESERVER Shared\Reports
3. Planted?            Q3.lnk to attacker UNC
4. Result?             victim NTLM auth to attacker
5. ATT&CK?             T1080
```

## How to solve it

1. Extract the evidence, password `hidden`: `7z x <sha256>.zip -phidden`. You get `taint-shared-content.pcap`.
2. Open `taint-shared-content.pcap` in Wireshark.
3. In Wireshark, paste this into the display filter bar and press Enter: `tcp.port==445 || ntlmssp`  (the lnk write and the victim auth). Then Statistics > Conversations for the talkers.
4. Same filter from the shell: `tshark -r taint-shared-content.pcap -Y "tcp.port==445 || ntlmssp"`, and `tshark -r taint-shared-content.pcap -q -z conv,ip`.
5. Confirm against the answer: A .lnk or similar written to a shared folder

## MITRE ATT&CK

- T1080 Taint Shared Content
- T1187 Forced Authentication

## Tools

Wireshark, tshark, zeek, tcpdump.

## Signature

Forensic Sample Blueprint | qayssarayra.com
