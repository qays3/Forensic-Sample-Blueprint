# Leak Site Upload

## Scenario

Stolen data is uploaded to an extortion leak site before publication: a very large upload to a leak portal host, the double extortion prep step.

Evidence: `leak-site-upload.pcap`  
Capture date: 2019-08-14

## Timeline

~14:01  A very large upload to a leak portal host, archive named after the victim.

## How to spot it

- A very large upload to an unknown host
- Leak portal hostname
- Archive named after the victim
- Prep for public extortion

## Questions

```
1. Technique?         Extortion leak prep
2. Destination?         leak portal host
3. Size?               very large upload
4. Stage?               before publication
5. ATT&CK?             T1567
```

## How to solve it

1. Extract the evidence, password `hidden`: `7z x <sha256>.zip -phidden`. You get `leak-site-upload.pcap`.
2. Open `leak-site-upload.pcap` in Wireshark.
3. In Wireshark, paste this into the display filter bar and press Enter: `ip.addr==185.100.87.202`  (the large upload). Then Statistics > Conversations for the talkers.
4. Same filter from the shell: `tshark -r leak-site-upload.pcap -Y "ip.addr==185.100.87.202"`, and `tshark -r leak-site-upload.pcap -q -z conv,ip`.
5. Confirm against the answer: A very large upload to an unknown host

## MITRE ATT&CK

- T1567 Exfiltration Over Web Service

## Tools

Wireshark, tshark, zeek, tcpdump.

## Signature

Forensic Sample Blueprint | qayssarayra.com
