# Repo Clone Exfil

## Scenario

An internal Git repository is cloned in bulk from 10.10.1.66 against 10.10.2.75: a git-upload-pack clone pulling the full history of a private repo.

Evidence: `repo-clone-exfil.pcap`  
Capture date: 2019-03-28

## Timeline

~12:00  A git clone of an internal repo, a large packfile transferred.

## How to spot it

- git-upload-pack clone operation
- Full repository history pulled
- Large packfile transfer
- Internal source code leaving via clone

## Questions

```
1. Protocol?          Git over HTTPS
2. Operation?           clone (upload-pack)
3. Repo?               internal-app.git
4. Volume?             full history packfile
5. ATT&CK?             T1213.003
```

## How to solve it

1. Extract the evidence, password `hidden`: `7z x <sha256>.zip -phidden`. You get `repo-clone-exfil.pcap`.
2. Open `repo-clone-exfil.pcap` in Wireshark.
3. In Wireshark, paste this into the display filter bar and press Enter: `tcp.port==443 && tls.handshake.extensions_server_name contains "git"`  (the clone). Then Statistics > Conversations for the talkers.
4. Same filter from the shell: `tshark -r repo-clone-exfil.pcap -Y "tcp.port==443 && tls.handshake.extensions_server_name contains "git""`, and `tshark -r repo-clone-exfil.pcap -q -z conv,ip`.
5. Confirm against the answer: git-upload-pack clone operation

## MITRE ATT&CK

- T1213.003 Code Repositories

## Tools

Wireshark, tshark, zeek, tcpdump.

## Signature

Forensic Sample Blueprint | qayssarayra.com
