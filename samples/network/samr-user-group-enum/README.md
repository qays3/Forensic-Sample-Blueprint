# Samr User Group Enum

## Scenario

User and group enumeration from 10.10.1.66 against the DC 10.10.2.10: the SAMR DCERPC interface is used to list domain users and groups (net user, net group).

Evidence: `samr-user-group-enum.pcap`  
Capture date: 2020-08-26

## Timeline

~11:00  SAMR DCERPC over IPC$, enumerate users and groups in the domain.

## How to spot it

- SAMR over DCERPC
- SamrEnumerateUsersInDomain and GroupsInDomain
- User and group lists returned
- Account discovery against the DC

## Questions

```
1. Interface?          SAMR
2. Calls?              SamrEnumerateUsers/Groups
3. Target?             domain controller
4. Goal?               list users and groups
5. ATT&CK?             T1087.002
```

## How to solve it

1. Extract the evidence, password `hidden`: `7z x <sha256>.zip -phidden`. You get `samr-user-group-enum.pcap`.
2. Open `samr-user-group-enum.pcap` in Wireshark.
3. In Wireshark, paste this into the display filter bar and press Enter: `dcerpc || tcp.port==445`  (the SAMR enumeration). Then Statistics > Conversations for the talkers.
4. Same filter from the shell: `tshark -r samr-user-group-enum.pcap -Y "dcerpc || tcp.port==445"`, and `tshark -r samr-user-group-enum.pcap -q -z conv,ip`.
5. Confirm against the answer: SAMR over DCERPC

## MITRE ATT&CK

- T1087.002 Domain Account
- T1069.002 Domain Groups

## Tools

Wireshark, tshark, zeek, tcpdump.

## Signature

Forensic Sample Blueprint | qayssarayra.com
