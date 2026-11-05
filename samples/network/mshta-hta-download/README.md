# Mshta Hta Download

## Scenario

mshta.exe fetches and runs a remote HTA file: an HTTP GET for a .hta served from an external host.

Evidence: `mshta-hta-download.pcap`  
Capture date: 2023-04-17

## Timeline

~11:00  HTTP GET for a .hta, HTML Application returned and executed by mshta.

## How to spot it

- HTTP GET for a .hta file
- Served as application/hta or text/html
- HTA contains WScript or ActiveX script
- Fetched by mshta.exe

## Questions

```
1. Technique?         mshta HTA exec
2. Server?             185.244.25.98
3. File?               /update.hta
4. Runner?             mshta.exe
5. ATT&CK?             T1218.005
```

## How to solve it

1. Extract the evidence, password `hidden`: `7z x <sha256>.zip -phidden`. You get `mshta-hta-download.pcap`.
2. Open `mshta-hta-download.pcap` in Wireshark.
3. In Wireshark, paste this into the display filter bar and press Enter: `http.request.uri contains ".hta"`  (the HTA fetch). Then Statistics > Conversations for the talkers.
4. Same filter from the shell: `tshark -r mshta-hta-download.pcap -Y "http.request.uri contains ".hta""`, and `tshark -r mshta-hta-download.pcap -q -z conv,ip`.
5. Confirm against the answer: HTTP GET for a .hta file

## MITRE ATT&CK

- T1218.005 Mshta

## Tools

Wireshark, tshark, zeek, tcpdump.

## Signature

Forensic Sample Blueprint | qayssarayra.com
