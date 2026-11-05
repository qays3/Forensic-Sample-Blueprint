# Domain Admins Enum

## Scenario

Enumeration of Domain Admins from 10.10.1.66 against the DC 10.10.2.10: LDAP queries for the Domain Admins group membership and its members.

Evidence: `domain-admins-enum.pcap`  
Capture date: 2025-11-19

## Timeline

~11:11  LDAP queries for the Domain Admins group and its member accounts.

## How to spot it

- LDAP query for the Domain Admins group
- Member attribute requested
- memberOf filter for privileged users
- Targeting the highest privilege group

## Questions

```
1. Protocol?          LDAP (389)
2. Target group?       Domain Admins
3. Attribute?          member / memberOf
4. Goal?               find domain admins
5. ATT&CK?             T1069.002
```

## How to solve it

1. Extract the evidence, password `hidden`: `7z x <sha256>.zip -phidden`. You get `domain-admins-enum.pcap`.
2. Open `domain-admins-enum.pcap` in Wireshark.
3. In Wireshark, paste this into the display filter bar and press Enter: `ldap`  (the Domain Admins query). Then Statistics > Conversations for the talkers.
4. Same filter from the shell: `tshark -r domain-admins-enum.pcap -Y "ldap"`, and `tshark -r domain-admins-enum.pcap -q -z conv,ip`.
5. Confirm against the answer: LDAP query for the Domain Admins group

## MITRE ATT&CK

- T1069.002 Domain Groups
- T1087.002 Domain Account

## Tools

Wireshark, tshark, zeek, tcpdump.

## Signature

Forensic Sample Blueprint | qayssarayra.com
