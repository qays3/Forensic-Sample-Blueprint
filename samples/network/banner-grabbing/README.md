# Banner Grabbing

## Scenario

10.10.1.66 connected briefly to several services on 10.10.2.18, read each service banner, then dropped the connection, classic banner grabbing for version detection.

Evidence: `banner-grabbing.pcap`  
Capture date: 2024-05-08

## Timeline

~13:06  Short connects to FTP 21, SSH 22, Telnet 23, SMTP 25, POP3 110; banner read then RST.

## How to spot it

- Connect, read banner, immediate RST on each port
- Service and version strings captured
- No authentication attempted
- One source touching many service ports

## Questions

```
1. Scanner?           10.10.1.66
2. Target?             10.10.2.18
3. Services?            FTP, SSH, Telnet, SMTP, POP3
4. Goal?               Software versions
5. Technique?          Banner grabbing
```

## How to solve it

1. Extract the evidence, password `hidden`: `7z x 86d599e391aa9760b97acb3496ea681dbaac35a9fea09e4b205a17bd3dbe28de.zip -phidden`. You get `banner-grabbing.pcap`.
2. Open `banner-grabbing.pcap` in Wireshark.
3. In Wireshark, paste this into the display filter bar and press Enter: `ip.addr==10.10.1.66`  (traffic to and from the attacker host). Then Statistics > Conversations to see the talkers.
4. Same filter from the shell with tshark: `tshark -r banner-grabbing.pcap -Y "ip.addr==10.10.1.66"`, and list talkers with `tshark -r banner-grabbing.pcap -q -z conv,ip`.
5. Confirm against the answer: Connect, read banner, immediate RST on each port

## MITRE ATT&CK

- T1592.002 Software
- T1046 Network Service Discovery

## Tools

Wireshark, tshark, zeek, tcpdump. Filter by the scanner IP to isolate the activity.

## Signature

Forensic Sample Blueprint | qayssarayra.com
