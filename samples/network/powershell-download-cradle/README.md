# Powershell Download Cradle

## Scenario

A PowerShell download cradle: the host fetches a remote script over HTTP and runs it in memory, the classic IEX DownloadString pattern.

Evidence: `powershell-download-cradle.pcap`  
Capture date: 2023-01-04

## Timeline

~10:00  HTTP GET for a .ps1 from an external IP, script body returned.

## How to spot it

- HTTP GET for a .ps1 or raw text payload
- A PowerShell or WebClient user agent
- Script fetched from a bare IP
- Downloaded content run in memory

## Questions

```
1. Technique?         PS download cradle
2. Server?             45.77.65.211
3. File?               /stage1.ps1
4. Pattern?            IEX DownloadString
5. ATT&CK?             T1059.001
```

## How to solve it

1. Extract the evidence, password `hidden`: `7z x <sha256>.zip -phidden`. You get `powershell-download-cradle.pcap`.
2. Open `powershell-download-cradle.pcap` in Wireshark.
3. In Wireshark, paste this into the display filter bar and press Enter: `http.request && http.request.uri contains ".ps1"`  (the script fetch). Then Statistics > Conversations for the talkers.
4. Same filter from the shell: `tshark -r powershell-download-cradle.pcap -Y "http.request && http.request.uri contains ".ps1""`, and `tshark -r powershell-download-cradle.pcap -q -z conv,ip`.
5. Confirm against the answer: HTTP GET for a .ps1 or raw text payload

## MITRE ATT&CK

- T1059.001 PowerShell
- T1105 Ingress Tool Transfer

## Tools

Wireshark, tshark, zeek, tcpdump.

## Signature

Forensic Sample Blueprint | qayssarayra.com
