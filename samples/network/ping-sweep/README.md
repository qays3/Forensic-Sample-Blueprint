# ping-sweep

## Scenario
10.10.1.66 ran an ICMP ping sweep across 10.10.2.1-59 to find live hosts.  

## How to spot it
- One host sends ICMP echo requests to a whole subnet in order  
- Live hosts reply with echo reply, dead ones stay silent  

## Questions
```
1. Scanner IP?       10.10.1.66
2. Method?           ICMP echo (ping) sweep
3. Range swept?      10.10.2.1 to 10.10.2.59
4. How many live?    9
```

## MITRE ATT&CK
T1018 Remote System Discovery  

## Signature
Forensic Sample Blueprint  |  qayssarayra.com
