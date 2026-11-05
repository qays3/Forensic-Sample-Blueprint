# Rmm Tool Traffic

## Scenario

Ongoing RMM tool traffic (AnyDesk, TeamViewer, Atera style): a persistent outbound session to a remote access vendor relay with regular keepalives.

Evidence: `rmm-tool-traffic.pcap`  
Capture date: 2022-11-27

## Timeline

~11:00  A persistent TLS session to a remote access vendor relay with steady keepalives.

## How to spot it

- A persistent session to a remote access vendor relay
- Vendor specific relay domain in SNI
- Regular keepalive packets
- Remote access software running in the environment

## Questions

```
1. Tool type?         RMM / remote access
2. Relay?              relay.anydesk-net.example
3. Pattern?            persistent + keepalives
4. Concern?             unsanctioned remote access
5. ATT&CK?             T1219
```

## How to solve it

1. Extract the evidence, password `hidden`: `7z x <sha256>.zip -phidden`. You get `rmm-tool-traffic.pcap`.
2. Open `rmm-tool-traffic.pcap` in Wireshark.
3. In Wireshark, paste this into the display filter bar and press Enter: `tls`  (the vendor relay session). Then Statistics > Conversations for the talkers.
4. Same filter from the shell: `tshark -r rmm-tool-traffic.pcap -Y "tls"`, and `tshark -r rmm-tool-traffic.pcap -q -z conv,ip`.
5. Confirm against the answer: A persistent session to a remote access vendor relay

## MITRE ATT&CK

- T1219 Remote Access Software

## Tools

Wireshark, tshark, zeek, tcpdump.

## Signature

Forensic Sample Blueprint | qayssarayra.com
