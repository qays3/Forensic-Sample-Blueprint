# xmas-null-fin-scans

## Scenario
10.10.1.66 ran three stealth scans against 10.10.2.15: Xmas (FIN+PSH+URG), NULL (no flags)  
and FIN (FIN only).  

## How to spot it
- TCP packets with odd or empty flag combos, never a normal SYN  
- Closed ports reply RST, open or filtered ports stay silent  
- Three waves, one per scan type  

## Questions
```
1. Scanner IP?        10.10.1.66
2. Three scan types?  Xmas, NULL, FIN
3. Xmas flags?        FIN, PSH, URG set together
4. Closed reply?      RST
```

## MITRE ATT&CK
T1046 Network Service Discovery  

## Signature
Forensic Sample Blueprint  |  qayssarayra.com
