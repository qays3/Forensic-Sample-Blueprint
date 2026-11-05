# dns-spoofing

## Scenario
An attacker (10.10.1.66) on the path forges DNS answers for internal names (login.corp.local), pointing the victim (10.10.1.50) at the attacker IP before the real DNS server replies.

## Timeline (UTC+3)
```
09:35:00  Victim queries login.corp.local
09:35:00  A forged answer (from the attacker) arrives first, pointing at 10.10.1.66
```

## How to spot it
- Two answers for one DNS query, or an answer from an odd source
- The forged answer resolves an internal name to the attacker
- Fast reply that beats the real DNS server
- Matched transaction ID but spoofed content

## Questions
```
1. Victim?           10.10.1.50
2. Attacker?         10.10.1.66
3. Names spoofed?    login.corp.local, intranet.corp.local
4. Spoofed answer?   Points to 10.10.1.66
5. How it wins?      Forged reply arrives first
```

## How to solve it

1. Extract the evidence, password `hidden`: `7z x 345f1059f109c881198e7fdfc153aa9e987c4928ce6048fd5f3b77912728a077.zip -phidden`. You get `dns-spoofing.pcap`.
2. Open `dns-spoofing.pcap` in Wireshark.
3. In Wireshark, paste this into the display filter bar and press Enter: `dns`  (DNS queries and answers). Then Statistics > Conversations to see the talkers.
4. Same filter from the shell with tshark: `tshark -r dns-spoofing.pcap -Y "dns"`, and list talkers with `tshark -r dns-spoofing.pcap -q -z conv,ip`.
5. Confirm against the answer: Two answers for one DNS query, or an answer from an odd source

## MITRE ATT&CK
- T1557 Adversary in the Middle
- T1565.001 Data Manipulation

## Tools
Crafted DNS spoofing (scapy) over generated background traffic

## Signature
Forensic Sample Blueprint | qayssarayra.com
