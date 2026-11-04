# dns-amplification

## Scenario
An attacker spoofs the victim's IP (10.10.2.5) and sends small DNS ANY queries to open resolvers. The resolvers send large answers to the victim, amplifying the attack many times over.

## Timeline (UTC+3)
```
10:25:00  Spoofed DNS ANY queries sent to public resolvers as 10.10.2.5
10:25:xx  Large DNS answers flood the victim, many times the query size
```

## How to spot it
- Small DNS ANY queries with the victim as the spoofed source
- Very large DNS answers all arriving at the victim
- Answers are many times bigger than the queries (amplification)
- Reflectors are open resolvers, the victim never asked

## Questions
```
1. Victim (spoofed)?  10.10.2.5
2. Technique?         DNS amplification/reflection
3. Query type?        ANY (large answers)
4. Reflectors?        Open DNS resolvers
5. Why amplified?     Answer >> query size
```

## MITRE ATT&CK
- T1498.002 Network Denial of Service: Reflection Amplification

## Tools
Crafted DNS amplification (scapy) over generated background traffic

## Signature
Forensic Sample Blueprint | qayssarayra.com
