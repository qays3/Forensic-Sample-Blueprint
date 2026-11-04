# idle-zombie-scan

## Scenario
10.10.1.66 ran an idle (zombie) scan against 10.10.2.15, using the quiet host 10.10.1.33  
as a zombie to hide its own address.  

## How to spot it
- The attacker never sends SYN to the target directly  
- It watches the zombie IP ID field, which jumps when the target replies to spoofed SYNs  
- Spoofed SYNs to the target appear to come from the zombie  

## Questions
```
1. Real attacker?     10.10.1.66
2. Zombie host?       10.10.1.33
3. Target?            10.10.2.15
4. What is watched?   The zombie IP ID increments
```

## MITRE ATT&CK
T1595.001 Active Scanning  

## Signature
Forensic Sample Blueprint  |  qayssarayra.com
