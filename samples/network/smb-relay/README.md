# smb-relay

## Scenario
An attacker (10.10.1.66) relays an SMB authentication from a victim (10.10.1.50) to another server (10.10.2.22) where SMB signing is off, gaining access as the victim without cracking anything.

## Timeline (UTC+3)
```
10:10:00  Victim authenticates to attacker over SMB
10:10:00  Attacker relays the same NTLM auth to 10.10.2.22 (signing off)
```

## How to spot it
- The same NTLM authentication appears in two SMB sessions
- The attacker sits between victim and target
- SMB signing is not required on the target
- No password cracking needed, the auth is reused

## Questions
```
1. Victim?           10.10.1.50
2. Relay?            10.10.1.66
3. Target?           10.10.2.22
4. Why possible?     SMB signing off on the target
5. Technique?         SMB relay
```

## How to solve it

1. Extract the evidence, password `hidden`: `7z x c245a9066777425d2c66b85c0271b15602a99697931b1d3943deff3d7a551f3d.zip -phidden`. You get `smb-relay.pcap`.
2. Open `smb-relay.pcap` in Wireshark.
3. In Wireshark, paste this into the display filter bar and press Enter: `smb2 || ntlmssp || smb`  (SMB and NTLM traffic). Then Statistics > Conversations to see the talkers.
4. Same filter from the shell with tshark: `tshark -r smb-relay.pcap -Y "smb2 || ntlmssp || smb"`, and list talkers with `tshark -r smb-relay.pcap -q -z conv,ip`.
5. Confirm against the answer: The same NTLM authentication appears in two SMB sessions

## MITRE ATT&CK
- T1557.001 Adversary in the Middle
- T1021.002 SMB/Windows Admin Shares

## Tools
Crafted SMB relay (scapy) over generated background traffic

## Signature
Forensic Sample Blueprint | qayssarayra.com
