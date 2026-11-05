# Ssh Lateral

## Scenario

SSH lateral movement from 10.10.1.66 to a Linux host 10.10.2.50: the SSH version exchange and key exchange on port 22, then an encrypted session.

Evidence: `ssh-lateral.pcap`  
Capture date: 2022-03-04

## Timeline

~12:00  SSH banner exchange on 22, key exchange, then encrypted session traffic.

## How to spot it

- Connection to SSH port 22
- SSH version banners exchanged
- Key exchange then encrypted traffic
- Operator host to an internal Linux host

## Questions

```
1. Protocol?          SSH (22)
2. Target?             10.10.2.50 (Linux)
3. Tell?               SSH banner + KEX
4. Source?             operator host
5. ATT&CK?             T1021.004
```

## How to solve it

1. Extract the evidence, password `hidden`: `7z x <sha256>.zip -phidden`. You get `ssh-lateral.pcap`.
2. Open `ssh-lateral.pcap` in Wireshark.
3. In Wireshark, paste this into the display filter bar and press Enter: `tcp.port==22`  (the SSH session). Then Statistics > Conversations for the talkers.
4. Same filter from the shell: `tshark -r ssh-lateral.pcap -Y "tcp.port==22"`, and `tshark -r ssh-lateral.pcap -q -z conv,ip`.
5. Confirm against the answer: Connection to SSH port 22

## MITRE ATT&CK

- T1021.004 SSH

## Tools

Wireshark, tshark, zeek, tcpdump.

## Signature

Forensic Sample Blueprint | qayssarayra.com
