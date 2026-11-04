# masscan-high-rate

## Scenario
10.10.1.66 ran a masscan style high rate scan across 10.10.0.0/16, firing thousands of SYN  
packets per second from a fixed source port.  

## How to spot it
- Extremely high packet rate from one host  
- Fixed source port (61000) and fixed window (1024), hallmarks of masscan  
- SYN packets sprayed across many hosts and a few common ports  

## Questions
```
1. Scanner IP?        10.10.1.66
2. Tool style?        masscan (high rate SYN)
3. Fixed source port? 61000
4. Tell from nmap?    Much higher rate, fixed source port
```

## MITRE ATT&CK
T1595.001 Active Scanning: Scanning IP Blocks  

## Signature
Forensic Sample Blueprint  |  qayssarayra.com
