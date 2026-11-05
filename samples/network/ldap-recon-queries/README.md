# Ldap Recon Queries

## Scenario

Targeted LDAP recon from 10.10.1.66 against 10.10.2.10: queries for adminCount=1 and SPN accounts to find privileged users and kerberoastable services.

Evidence: `ldap-recon-queries.pcap`  
Capture date: 2025-01-11

## Timeline

~11:11  LDAP queries filtering adminCount=1 and servicePrincipalName=*.

## How to spot it

- LDAP filters for adminCount=1
- LDAP filters for servicePrincipalName
- Looking for admins and SPN accounts
- Expensive or unusual LDAP searches

## Questions

```
1. Protocol?          LDAP (389)
2. Filters?            adminCount=1, SPN=*
3. Goal?               admins and kerberoastable accounts
4. Target?             DC
5. Technique?          Account discovery
```

## How to solve it

1. Extract the evidence, password `hidden`: `7z x <sha256>.zip -phidden`. You get `ldap-recon-queries.pcap`.
2. Open `ldap-recon-queries.pcap` in Wireshark.
3. In Wireshark, paste this into the display filter bar and press Enter: `ldap`  (the LDAP filters). Then Statistics > Conversations for the talkers.
4. Same filter from the shell: `tshark -r ldap-recon-queries.pcap -Y "ldap"`, and `tshark -r ldap-recon-queries.pcap -q -z conv,ip`.
5. Confirm against the answer: LDAP filters for adminCount=1

## MITRE ATT&CK

- T1087.002 Domain Account
- T1018 Remote System Discovery

## Tools

Wireshark, tshark, zeek, tcpdump.

## Signature

Forensic Sample Blueprint | qayssarayra.com
