# Ingress Tool Transfer

## Scenario

A tool is pulled from attacker infrastructure over HTTP: a curl user agent downloads an .exe (MZ) from a drop host.

Evidence: `ingress-tool-transfer.pcap`  
Capture date: 2022-01-08

## Timeline

~11:11  curl GET of a.exe from files-drop-corp.xyz returning an MZ binary.

## How to spot it

- A download of an .exe from a new external host
- curl or non browser user agent
- Response body starts with MZ
- Followed by execution on the host

## Questions

```
1. File?              a.exe
2. Source?             files-drop-corp.xyz
3. Agent?              curl
4. Body?               MZ binary
5. Technique?          Ingress tool transfer
```

## How to solve it

1. Extract the evidence, password `hidden`: `7z x <sha256>.zip -phidden`. You get `ingress-tool-transfer.pcap`.
2. Open `ingress-tool-transfer.pcap` in Wireshark.
3. In Wireshark, paste this into the display filter bar and press Enter: `http.request.uri contains ".exe"`  (The tool download). Then Statistics > Conversations for the talkers.
4. Same filter from the shell: `tshark -r ingress-tool-transfer.pcap -Y "http.request.uri contains ".exe""`, and `tshark -r ingress-tool-transfer.pcap -q -z conv,ip`.
5. Confirm against the answer: A download of an .exe from a new external host

## MITRE ATT&CK

- T1105 Ingress Tool Transfer

## Tools

Wireshark, tshark, zeek, tcpdump.

## Signature

Forensic Sample Blueprint | qayssarayra.com
