# os-fingerprinting

## Scenario
10.10.1.66 sent crafted TCP probes with unusual TTL, window size and option orders to  
10.10.2.15 to fingerprint the operating system.  

## How to spot it
- A few hand crafted SYN packets with odd TTL (64, 128, 255), window and TCP options  
- The attacker reads the replies to guess the OS  
- Very low packet count compared with a port scan  

## Questions
```
1. Scanner IP?        10.10.1.66
2. Technique?         OS fingerprinting from TTL, window, TCP options
3. Target reply TTL?  64 (Linux like)
```

## MITRE ATT&CK
T1592.002 Gather Victim Host Information: Software  

## Signature
Forensic Sample Blueprint  |  qayssarayra.com
