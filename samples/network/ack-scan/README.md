# ack-scan

## Scenario
10.10.1.66 ran an ACK scan against 10.10.2.15 to map which ports a firewall filters.  

## How to spot it
- Bare ACK packets with no prior handshake  
- Unfiltered ports reply RST, filtered ports (139, 445) give no reply  
- Used to learn firewall rules, not open or closed state  

## Questions
```
1. Scanner IP?          10.10.1.66
2. Scan goal?           Map firewall filtering
3. Unfiltered sign?     RST reply
4. Filtered ports?      139, 445 (no reply)
```

## MITRE ATT&CK
T1046 Network Service Discovery  

## Signature
Forensic Sample Blueprint  |  qayssarayra.com
