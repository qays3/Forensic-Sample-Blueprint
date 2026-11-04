# udp-scan

## Scenario
10.10.1.66 ran a UDP scan of ports 1-499 on 10.10.2.15.  

## How to spot it
- UDP probes to many ports from one host  
- Closed UDP ports answer ICMP type 3 code 3 (port unreachable)  
- Ports with no reply may be open or filtered (here 53, 123, 161 stay silent)  

## Questions
```
1. Scanner IP?         10.10.1.66
2. Protocol?           UDP
3. Closed port sign?   ICMP port unreachable (type 3 code 3)
4. Likely open?        53, 123, 161 (no reply)
```

## MITRE ATT&CK
T1046 Network Service Discovery  

## Signature
Forensic Sample Blueprint  |  qayssarayra.com
