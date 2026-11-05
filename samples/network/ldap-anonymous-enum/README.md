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

## MITRE ATT&CK

- T1087.002 Domain Account
- T1590 Gather Victim Network Information

## Tools

Wireshark, tshark, zeek, tcpdump. Filter by the scanner IP to isolate the activity.

## Signature

Forensic Sample Blueprint | qayssarayra.com
