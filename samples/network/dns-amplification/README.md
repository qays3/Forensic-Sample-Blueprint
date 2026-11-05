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

## How to solve it

1. Extract the evidence, password `hidden`: `7z x eb3f368f401dfe6cc0d638acbf21254614ad3358d3f84d81518a9b3a6b7d6cac.zip -phidden`. You get `dns-amplification.pcap`.
2. Open `dns-amplification.pcap` in Wireshark.
3. In Wireshark, paste this into the display filter bar and press Enter: `dns`  (DNS queries and answers). Then Statistics > Conversations to see the talkers.
4. Same filter from the shell with tshark: `tshark -r dns-amplification.pcap -Y "dns"`, and list talkers with `tshark -r dns-amplification.pcap -q -z conv,ip`.
5. Confirm against the answer: Small DNS ANY queries with the victim as the spoofed source

## MITRE ATT&CK
- T1498.002 Network Denial of Service: Reflection Amplification

## Tools
Crafted DNS amplification (scapy) over generated background traffic

## Signature
Forensic Sample Blueprint | qayssarayra.com
