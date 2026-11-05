# Sharphound Ldap Burst

## Scenario

SharpHound collection from 10.10.1.66 against the domain controller 10.10.2.10: a large burst of LDAP searches pulling users, groups, computers, and ACLs.

Evidence: `sharphound-ldap-burst.pcap`  
Capture date: 2022-12-26

## Timeline

~12:10  Dozens of LDAP searches to the DC in a short window, pulling directory objects.

## How to spot it

- A large burst of LDAP searches to the DC
- Queries for users, groups, SPNs and ACLs
- One host, many queries, short window
- BloodHound collection pattern

## Questions

```
1. Tool?              SharpHound
2. Protocol?           LDAP (389)
3. Target?             domain controller
4. Tell?               huge LDAP burst
5. Technique?          Domain discovery
```

## How to solve it

1. Extract the evidence, password `hidden`: `7z x <sha256>.zip -phidden`. You get `sharphound-ldap-burst.pcap`.
2. Open `sharphound-ldap-burst.pcap` in Wireshark.
3. In Wireshark, paste this into the display filter bar and press Enter: `ldap || tcp.port==389`  (the LDAP burst). Then Statistics > Conversations for the talkers.
4. Same filter from the shell: `tshark -r sharphound-ldap-burst.pcap -Y "ldap || tcp.port==389"`, and `tshark -r sharphound-ldap-burst.pcap -q -z conv,ip`.
5. Confirm against the answer: A large burst of LDAP searches to the DC

## MITRE ATT&CK

- T1087 Account Discovery
- T1069 Permission Groups Discovery

## Tools

Wireshark, tshark, zeek, tcpdump.

## Signature

Forensic Sample Blueprint | qayssarayra.com
