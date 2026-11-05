# Web Dir Brute Force

## Scenario

Web directory and file brute forcing against 10.10.2.80: a scanner requests hundreds of common paths, most 404, a few 200, to map hidden content.

Evidence: `web-dir-brute-force.pcap`  
Capture date: 2020-02-20

## Timeline

~10:00  Hundreds of HTTP requests for common paths, mostly 404 with a few 200.

## How to spot it

- Hundreds of HTTP requests in a short window
- A scanner user agent (gobuster, ffuf, dirb)
- Mostly 404 with occasional 200
- Sequential or wordlist style paths

## Questions

```
1. Activity?          Web content brute force
2. Target?             10.10.2.80
3. Tool tell?          gobuster user agent
4. Pattern?            many 404, few 200
5. ATT&CK?             T1595.003
```

## How to solve it

1. Extract the evidence, password `hidden`: `7z x <sha256>.zip -phidden`. You get `web-dir-brute-force.pcap`.
2. Open `web-dir-brute-force.pcap` in Wireshark.
3. In Wireshark, paste this into the display filter bar and press Enter: `http.response.code==404`  (the 404 storm). Then Statistics > Conversations for the talkers.
4. Same filter from the shell: `tshark -r web-dir-brute-force.pcap -Y "http.response.code==404"`, and `tshark -r web-dir-brute-force.pcap -q -z conv,ip`.
5. Confirm against the answer: Hundreds of HTTP requests in a short window

## MITRE ATT&CK

- T1595.003 Wordlist Scanning

## Tools

Wireshark, tshark, zeek, tcpdump.

## Signature

Forensic Sample Blueprint | qayssarayra.com
