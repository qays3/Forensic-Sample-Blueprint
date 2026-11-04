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

## MITRE ATT&CK
T1595.001 Active Scanning  
T1046 Network Service Discovery  

## Signature
Forensic Sample Blueprint  |  qayssarayra.com
