# c2-over-telegram

## Scenario
A host (10.10.1.68) uses the Telegram Bot API as its command and control. It polls api.telegram.org with getUpdates from a python-requests agent, so the traffic blends with a trusted service while the bot carries commands.

## Timeline (UTC+3)
```
09:18:20  First getUpdates poll to api.telegram.org (149.154.167.220)
all day   Polls about every 50s, ~413 of them
```

## How to spot it
- Requests to a legit API (Telegram) but from a non browser (python-requests)
- Steady polling of getUpdates, the bot command channel
- A trusted destination, so it is easy to miss
- An office workstation calling the Telegram bot API is odd

## Questions
```
1. Host?             10.10.1.68
2. Service abused?   Telegram Bot API (api.telegram.org)
3. Endpoint?         /bot<token>/getUpdates
4. Client?           python-requests, not a browser
5. Why it hides?     Trusted service destination
```

## How to solve it

1. Extract the evidence, password `hidden`: `7z x 2c3ec541fc4c7dee9fe163f97f38e31d4911977c3d43ebfb66fbc18bf288f938.zip -phidden`. You get `c2-over-telegram.pcap`.
2. Open `c2-over-telegram.pcap` in Wireshark.
3. In Wireshark, paste this into the display filter bar and press Enter: `http || tls`  (HTTP/TLS requests, hosts and user agents). Then Statistics > Conversations to see the talkers.
4. Same filter from the shell with tshark: `tshark -r c2-over-telegram.pcap -Y "http || tls"`, and list talkers with `tshark -r c2-over-telegram.pcap -q -z conv,ip`.
5. Confirm against the answer: Requests to a legit API (Telegram) but from a non browser (python-requests)

## MITRE ATT&CK
- T1102.002 Web Service: Bidirectional Communication
- T1071.001 Web Protocols

## Tools
Crafted Telegram bot API C2 (scapy) over generated background traffic

## Signature
Forensic Sample Blueprint | qayssarayra.com
