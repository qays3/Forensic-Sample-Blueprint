# Remote Access Tool Install

## Scenario

A remote access tool is installed and dials out to its relay: the client installer is downloaded, then an outbound session is established to the vendor relay for remote control.

Evidence: `remote-access-tool-install.pcap`  
Capture date: 2025-06-05

## Timeline

~10:01  Installer download for a remote support client, then an outbound session to its relay.

## How to spot it

- Download of a remote support client installer
- An outbound session to the tool's relay infrastructure
- Legitimate RMM software used for access
- Unexpected remote control tool in the environment

## Questions

```
1. Technique?         Remote access software
2. Tool host?           screenconnect-host.example
3. Relay?              relay.screenconnect-host.example
4. Use?                remote control
5. ATT&CK?             T1219
```

## How to solve it

1. Extract the evidence, password `hidden`: `7z x <sha256>.zip -phidden`. You get `remote-access-tool-install.pcap`.
2. Open `remote-access-tool-install.pcap` in Wireshark.
3. In Wireshark, paste this into the display filter bar and press Enter: `tls || http`  (the installer download and relay session). Then Statistics > Conversations for the talkers.
4. Same filter from the shell: `tshark -r remote-access-tool-install.pcap -Y "tls || http"`, and `tshark -r remote-access-tool-install.pcap -q -z conv,ip`.
5. Confirm against the answer: Download of a remote support client installer

## MITRE ATT&CK

- T1219 Remote Access Software

## Tools

Wireshark, tshark, zeek, tcpdump.

## Signature

Forensic Sample Blueprint | qayssarayra.com
