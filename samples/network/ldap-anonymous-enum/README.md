# Ldap Anonymous Enum

## Scenario

10.10.1.66 bound anonymously to the domain controller 10.10.2.10 on LDAP 389 and searched the directory without credentials.

Evidence: `ldap-anonymous-enum.pcap`  
Capture date: 2024-03-16

## Timeline

~13:03  Anonymous bind to LDAP, then a wholeSubtree user search returning directory entries.

## How to spot it

- An anonymous LDAP bindRequest (no credentials)
- A broad searchRequest over the whole subtree
- User and OU entries returned to an unauthenticated client
- Cleartext LDAP on 389

## Questions

```
1. Scanner?           10.10.1.66
2. Target?             10.10.2.10 (DC)
3. Bind?               Anonymous
4. Returned?            Users and OUs
5. Technique?          LDAP anonymous enum
```

## How to solve it

1. Extract the evidence, password `hidden`: `7z x b5806e4e860021db08bf1431397001c3fcdb01989617b777018144693f7d8f00.zip -phidden`. You get `ldap-anonymous-enum.pcap`.
2. Open `ldap-anonymous-enum.pcap` in Wireshark.
3. In Wireshark, paste this into the display filter bar and press Enter: `ldap`  (LDAP bind and search operations). Then Statistics > Conversations to see the talkers.
4. Same filter from the shell with tshark: `tshark -r ldap-anonymous-enum.pcap -Y "ldap"`, and list talkers with `tshark -r ldap-anonymous-enum.pcap -q -z conv,ip`.
5. Confirm against the answer: An anonymous LDAP bindRequest (no credentials)

## MITRE ATT&CK

- T1087.002 Domain Account
- T1590 Gather Victim Network Information

## Tools

Wireshark, tshark, zeek, tcpdump. Filter by the scanner IP to isolate the activity.

## Signature

Forensic Sample Blueprint | qayssarayra.com
