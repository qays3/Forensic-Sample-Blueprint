# asrep-roasting-traffic

## Scenario
An attacker host (10.10.1.66) sends Kerberos AS-REQ messages with no pre authentication for accounts that have 'do not require preauth' set. The domain controller 10.10.2.10 returns AS-REP messages whose encrypted part can be cracked offline.

## Timeline (UTC+3)
```
13:25:00  AS-REQ without preauth for svc-legacy to 10.10.2.10
13:25:xx  DC returns AS-REP with RC4 encrypted part
13:25:xx  Same for helpdesk and kiosk
```

## How to spot it
- AS-REQ messages carry no pre authentication timestamp
- Only accounts with preauth disabled get an AS-REP back
- The AS-REP encrypted part is RC4 and crackable offline
- Requests target specific weak accounts, not normal logons

## Questions
```
1. Attacker IP?         10.10.1.66
2. Domain controller?   10.10.2.10
3. What is missing?     Kerberos pre authentication
4. Accounts hit?        svc-legacy, helpdesk, kiosk
5. DC side event?       4768 with preauth type 0
```

## How to solve it

1. Extract the evidence, password `hidden`: `7z x 28efcdfb38a08c68b355266a540c707156698e602722ad944f04fcb05472eaf1.zip -phidden`. You get `asrep-roasting-traffic.pcap`.
2. Open `asrep-roasting-traffic.pcap` in Wireshark.
3. In Wireshark, paste this into the display filter bar and press Enter: `kerberos`  (Kerberos AS-REQ/TGS-REQ and encryption types). Then Statistics > Conversations to see the talkers.
4. Same filter from the shell with tshark: `tshark -r asrep-roasting-traffic.pcap -Y "kerberos"`, and list talkers with `tshark -r asrep-roasting-traffic.pcap -q -z conv,ip`.
5. Confirm against the answer: AS-REQ messages carry no pre authentication timestamp

## MITRE ATT&CK
- T1558.004 Steal or Forge Kerberos Tickets: AS-REP Roasting

## Tools
Crafted Kerberos AS exchange on port 88 (scapy) over generated background traffic. Payloads are representative markers.


## Note
This pcap is a representative sample built with scapy: ports and markers are correct but the payload bytes are placeholders, not full protocol data. A faithful capture of this attack needs a real Windows domain controller, which this lab (Samba) cannot reproduce.

## Signature
Forensic Sample Blueprint | qayssarayra.com
