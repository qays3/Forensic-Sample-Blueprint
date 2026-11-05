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

## MITRE ATT&CK

- T1135 Network Share Discovery
- T1087 Account Discovery

## Tools

Wireshark, tshark, zeek, tcpdump. Filter by the scanner IP to isolate the activity.

## Signature

Forensic Sample Blueprint | qayssarayra.com
