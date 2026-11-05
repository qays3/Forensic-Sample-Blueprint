# Drive By Download

## Scenario

A drive by download: visiting a page triggers a script that silently fetches and delivers a payload without the user clicking anything.

Evidence: `drive-by-download.pcap`  
Capture date: 2023-03-08

## Timeline

~11:00  A page load, a loader script, then an automatic payload fetch.

## How to spot it

- A normal page load followed by a loader script
- An automatic redirect or fetch to a payload
- No user click between page and download
- Payload delivered on visit

## Questions

```
1. Technique?         Drive by download
2. Site?               news-blog.example
3. Loader?             /js/loader.js
4. Payload?            /d/payload.msi
5. ATT&CK?             T1189
```

## How to solve it

1. Extract the evidence, password `hidden`: `7z x <sha256>.zip -phidden`. You get `drive-by-download.pcap`.
2. Open `drive-by-download.pcap` in Wireshark.
3. In Wireshark, paste this into the display filter bar and press Enter: `http`  (the loader and payload fetch). Then Statistics > Conversations for the talkers.
4. Same filter from the shell: `tshark -r drive-by-download.pcap -Y "http"`, and `tshark -r drive-by-download.pcap -q -z conv,ip`.
5. Confirm against the answer: A normal page load followed by a loader script

## MITRE ATT&CK

- T1189 Drive-by Compromise

## Tools

Wireshark, tshark, zeek, tcpdump.

## Signature

Forensic Sample Blueprint | qayssarayra.com
