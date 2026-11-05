# Smb Null Session Enum

## Scenario

10.10.1.66 opened a null (anonymous) SMB session to the IPC$ share on 10.10.2.12 and enumerated shares over the SRVSVC pipe.

Evidence: `smb-null-session-enum.pcap`  
Capture date: 2021-06-15

## Timeline

~12:05  Anonymous session setup to 445, tree connect to IPC$, NetShareEnumAll returns the share list.

## How to spot it

- An anonymous/null SMB session setup
- A tree connect to IPC$
- NetShareEnumAll over the srvsvc pipe
- Share names disclosed without credentials

## Questions

```
1. Scanner?           10.10.1.66
2. Target?             10.10.2.12
3. Share?              IPC$ (null session)
4. Disclosed?           C$, ADMIN$, Finance, HR
5. Technique?          SMB null session enum
```

## How to solve it

1. Extract the evidence, password `hidden`: `7z x 5bc01e6400a259a591c40b4e15d54c3aa13e8eb75fdd786120e1109fd956ee25.zip -phidden`. You get `smb-null-session-enum.pcap`.
2. Open `smb-null-session-enum.pcap` in Wireshark.
3. In Wireshark, paste this into the display filter bar and press Enter: `smb2 || ntlmssp || smb`  (SMB and NTLM traffic). Then Statistics > Conversations to see the talkers.
4. Same filter from the shell with tshark: `tshark -r smb-null-session-enum.pcap -Y "smb2 || ntlmssp || smb"`, and list talkers with `tshark -r smb-null-session-enum.pcap -q -z conv,ip`.
5. Confirm against the answer: An anonymous/null SMB session setup

## MITRE ATT&CK

- T1135 Network Share Discovery
- T1087 Account Discovery

## Tools

Wireshark, tshark, zeek, tcpdump. Filter by the scanner IP to isolate the activity.

## Signature

Forensic Sample Blueprint | qayssarayra.com
