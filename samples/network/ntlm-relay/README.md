# ntlm-relay

## Scenario
An attacker (10.10.1.66) receives an NTLM authentication from a victim (10.10.1.50) and relays it, unchanged, to a third server (10.10.2.22) that does not require SMB signing. The same challenge appears in both connections, and the attacker gains access to the target as the victim.

## Timeline (UTC+3)
```
11:25:00  Victim 10.10.1.50 authenticates to attacker 10.10.1.66 over SMB (NTLMSSP)
11:25:00  Attacker opens SMB to 10.10.2.22 and replays the same NTLM challenge
11:25:xx  Auth succeeds on the target, attacker writes to ADMIN$
```

## How to spot it
- Two SMB sessions share the exact same NTLM server challenge
- The attacker sits between the victim and the target
- SMB signing is off on the target, which is what allows the relay
- After auth the attacker writes a service binary to ADMIN$

## Questions
```
1. Victim IP?           10.10.1.50
2. Relay (attacker)?    10.10.1.66
3. Final target?        10.10.2.22
4. What links the two sessions?   The same NTLM challenge
5. Why possible?        SMB signing not required on the target
```

## How to solve it

1. Extract the evidence, password `hidden`: `7z x 607ccdf37ee8453da3cad036dc470664ea82c1992aad7ee65ebe36358c3d5e1f.zip -phidden`. You get `ntlm-relay.pcap`.
2. Open `ntlm-relay.pcap` in Wireshark.
3. In Wireshark, paste this into the display filter bar and press Enter: `smb2 || ntlmssp || smb`  (SMB and NTLM traffic). Then Statistics > Conversations to see the talkers.
4. Same filter from the shell with tshark: `tshark -r ntlm-relay.pcap -Y "smb2 || ntlmssp || smb"`, and list talkers with `tshark -r ntlm-relay.pcap -q -z conv,ip`.
5. Confirm against the answer: Two SMB sessions share the exact same NTLM server challenge

## MITRE ATT&CK
- T1557.001 Adversary in the Middle
- T1021.002 Remote Services: SMB/Windows Admin Shares

## Tools
Crafted NTLMSSP relay across two SMB sessions (scapy) over generated background traffic


## Note
This pcap is a representative sample built with scapy: ports and markers are correct but the payload bytes are placeholders, not full protocol data. A faithful capture of this attack needs a real Windows domain controller, which this lab (Samba) cannot reproduce.

## Signature
Forensic Sample Blueprint | qayssarayra.com
