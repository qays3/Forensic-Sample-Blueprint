# kerberoasting-traffic

## Scenario
Real capture against a live Active Directory domain controller (Samba AD, domain corp.local, DC 10.10.2.10). A domain user (alice, from 10.10.1.66) enumerates service principal names over LDAP, then requests the Kerberos service ticket for the SQL service account, forcing RC4 so the ticket can be cracked offline. This is genuine traffic, not crafted.

## Timeline (UTC+3)
```
Attacker 10.10.1.66 lists SPNs over LDAP (389) against DC 10.10.2.10
Attacker gets a TGT for alice (AS-REQ / AS-REP on 88)
Attacker requests the MSSQLSvc/sql01.corp.local:1433 service ticket (TGS-REQ / TGS-REP, RC4)
```

## How to spot it
- An LDAP query filtering for accounts that have a servicePrincipalName
- A TGS request for a service account ticket using RC4 (etype 23), which is weak
- The requesting user does not normally use that service
- On the DC this is event 4769 with ticket encryption type 0x17

## Questions
```
1. Requesting user and host?   alice from 10.10.1.66
2. Domain controller?          10.10.2.10 (corp.local)
3. Service account roasted?    svc_sql (SPN MSSQLSvc/sql01.corp.local:1433)
4. Encryption forced?          RC4 (etype 23)
5. Discovery step?             LDAP SPN enumeration before the ticket request
```

## How to solve it

1. Extract the evidence, password `hidden`: `7z x fa6b54609ce75f0b8aa66a398aeac777ac470d2e9d8141c61c8af83d0ba21ebb.zip -phidden`. You get `kerberoasting-traffic.pcap`.
2. Open `kerberoasting-traffic.pcap` in Wireshark.
3. In Wireshark, paste this into the display filter bar and press Enter: `kerberos`  (Kerberos AS-REQ/TGS-REQ and encryption types). Then Statistics > Conversations to see the talkers.
4. Same filter from the shell with tshark: `tshark -r kerberoasting-traffic.pcap -Y "kerberos"`, and list talkers with `tshark -r kerberoasting-traffic.pcap -q -z conv,ip`.
5. Confirm against the answer: An LDAP query filtering for accounts that have a servicePrincipalName

## MITRE ATT&CK
- T1558.003 Steal or Forge Kerberos Tickets: Kerberoasting

## Tools
Real capture: impacket GetUserSPNs for LDAP enumeration, native Kerberos client (kinit, kvno) for the ticket request, against a Samba AD DC. tcpdump on the link.


## Signature
Forensic Sample Blueprint | qayssarayra.com
