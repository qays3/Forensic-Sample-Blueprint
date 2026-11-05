# Msiexec Remote Install

## Scenario

msiexec installs an MSI package pulled from a remote URL: an HTTP GET for a .msi served from an external host.

Evidence: `msiexec-remote-install.pcap`  
Capture date: 2024-07-21

## Timeline

~12:00  HTTP GET for a .msi, package body returned and installed by msiexec.

## How to spot it

- HTTP GET for a .msi over HTTP
- Large package body
- MSI OLE compound header
- Installed by msiexec /i with a URL

## Questions

```
1. Technique?         msiexec remote install
2. Server?             139.59.22.170
3. File?               /pkg/setup.msi
4. Runner?             msiexec.exe
5. ATT&CK?             T1218.007
```

## How to solve it

1. Extract the evidence, password `hidden`: `7z x <sha256>.zip -phidden`. You get `msiexec-remote-install.pcap`.
2. Open `msiexec-remote-install.pcap` in Wireshark.
3. In Wireshark, paste this into the display filter bar and press Enter: `http.request.uri contains ".msi"`  (the MSI fetch). Then Statistics > Conversations for the talkers.
4. Same filter from the shell: `tshark -r msiexec-remote-install.pcap -Y "http.request.uri contains ".msi""`, and `tshark -r msiexec-remote-install.pcap -q -z conv,ip`.
5. Confirm against the answer: HTTP GET for a .msi over HTTP

## MITRE ATT&CK

- T1218.007 Msiexec

## Tools

Wireshark, tshark, zeek, tcpdump.

## Signature

Forensic Sample Blueprint | qayssarayra.com
