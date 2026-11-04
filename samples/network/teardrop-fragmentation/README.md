# teardrop-fragmentation

## Scenario
An attacker (10.10.1.66) sends overlapping, malformed IP fragments (teardrop) to 10.10.2.20. Buggy reassembly can crash or hang the target.

## Timeline (UTC+3)
```
10:30:00  Overlapping fragments with bad offsets sent to 10.10.2.20
10:30:xx  Repeated malformed fragment pairs
```

## How to spot it
- IP fragments with overlapping or impossible offsets
- Same IP id with fragments that cannot be reassembled cleanly
- Aimed at weak fragment reassembly code
- Can crash or hang older stacks

## Questions
```
1. Attacker?         10.10.1.66
2. Target?           10.10.2.20
3. Technique?         Overlapping IP fragments (teardrop)
4. Tell?             Bad/overlapping fragment offsets, same IP id
5. Effect?           Crash or hang on reassembly
```

## MITRE ATT&CK
- T1499.001 Endpoint Denial of Service: OS Exhaustion Flood

## Tools
Crafted teardrop fragments (scapy) over generated background traffic

## Signature
Forensic Sample Blueprint | qayssarayra.com
