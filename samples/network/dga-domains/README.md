# dga-domains

## Scenario
A host (10.10.1.91) is infected with malware that uses a domain generation algorithm. It looks up many long random domains to find its live C2. Most lookups fail (NXDOMAIN), a few resolve, which is the current C2.

## Timeline (UTC+3)
```
10:10:00  10.10.1.91 starts resolving long random domains
10:1x:xx  ~60 lookups, most NXDOMAIN, a few resolve
```

## How to spot it
- A burst of DNS lookups for long, random, meaningless domains
- A high rate of NXDOMAIN answers from one host
- Mixed TLDs and random labels, the hallmark of a DGA
- The few that resolve point to the active C2

## Questions
```
1. Host?             10.10.1.91
2. Technique?         Domain generation algorithm (DGA)
3. Main DNS answer?   NXDOMAIN for most
4. What matters?      The few domains that resolve (live C2)
5. Tell?              Long random labels, many failed lookups
```

## MITRE ATT&CK
- T1568.002 Dynamic Resolution: Domain Generation Algorithms

## Tools
Crafted DGA DNS lookups (scapy) over generated background traffic

## Signature
Forensic Sample Blueprint | qayssarayra.com
