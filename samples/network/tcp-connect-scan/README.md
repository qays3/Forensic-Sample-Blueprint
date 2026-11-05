# tcp-connect-scan

## Scenario
10.10.1.66 ran a TCP connect (full handshake) scan of ports 1-1024 on 10.10.2.15.  

## Timeline (UTC+3)
```
11:13:20  Connect scan starts from 10.10.1.66, source port 40296
```

## How to spot it
- Unlike a SYN scan, open ports get a COMPLETED handshake (SYN, SYN ACK, ACK) then RST  
- One source, 1024 ports, one target, in seconds  
- Closed ports reply RST ACK  

Open ports: 22, 80, 443, 445, 3389  

## Questions
```
1. Scanner IP?            10.10.1.66
2. Scan type?             TCP connect (full three way handshake)
3. Open ports?            22, 80, 443, 445, 3389
4. SYN vs connect tell?   Connect completes the handshake before RST
```

## How to solve it

1. Extract the evidence, password `hidden`: `7z x 6573c75d7a805d64e700c72ab8d1d0014cf56c469a11337a5cc9547d76eb65e2.zip -phidden`. You get `tcp-connect-scan.pcap`.
2. Open `tcp-connect-scan.pcap` in Wireshark.
3. In Wireshark, paste this into the display filter bar and press Enter: `tcp.flags.syn==1 && tcp.flags.ack==0`  (SYN probes across ports). Then Statistics > Conversations to see the talkers.
4. Same filter from the shell with tshark: `tshark -r tcp-connect-scan.pcap -Y "tcp.flags.syn==1 && tcp.flags.ack==0"`, and list talkers with `tshark -r tcp-connect-scan.pcap -q -z conv,ip`.
5. Confirm against the answer: Unlike a SYN scan, open ports get a COMPLETED handshake (SYN, SYN ACK, ACK) then RST

## MITRE ATT&CK
T1595.001 Active Scanning  
T1046 Network Service Discovery  

## Signature
Forensic Sample Blueprint  |  qayssarayra.com
