# Forced Auth Share

## Scenario

Forced authentication: a planted SCF or LNK file in a share points at an attacker SMB server 10.10.1.66, so a victim browsing the folder sends its NTLM authentication to the attacker.

Evidence: `forced-auth-share.pcap`  
Capture date: 2023-03-02

## Timeline

~12:00  The victim auto connects to an attacker SMB host and sends NTLMSSP auth after browsing a share.

## How to spot it

- Outbound SMB to an attacker host triggered by browsing a share
- NTLMSSP negotiate, challenge, auth sequence
- Victim sends its hash to an unexpected server
- Triggered by an SCF or LNK icon path

## Questions

```
1. Technique?         Forced authentication
2. Attacker host?       10.10.1.66
3. Trigger?            SCF/LNK in a share
4. Captured?           NTLM auth to attacker
5. ATT&CK?             T1187
```

## How to solve it

1. Extract the evidence, password `hidden`: `7z x <sha256>.zip -phidden`. You get `forced-auth-share.pcap`.
2. Open `forced-auth-share.pcap` in Wireshark.
3. In Wireshark, paste this into the display filter bar and press Enter: `ntlmssp || tcp.port==445`  (the NTLM auth to the attacker host). Then Statistics > Conversations for the talkers.
4. Same filter from the shell: `tshark -r forced-auth-share.pcap -Y "ntlmssp || tcp.port==445"`, and `tshark -r forced-auth-share.pcap -q -z conv,ip`.
5. Confirm against the answer: Outbound SMB to an attacker host triggered by browsing a share

## MITRE ATT&CK

- T1187 Forced Authentication

## Tools

Wireshark, tshark, zeek, tcpdump.

## Signature

Forensic Sample Blueprint | qayssarayra.com
