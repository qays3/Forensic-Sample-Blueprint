# Remote Template Injection

## Scenario

A document loads a remote Word template over HTTP (remote template injection): opening the document triggers a GET for a .dotm that carries the macro.

Evidence: `remote-template-injection.pcap`  
Capture date: 2022-03-22

## Timeline

~11:05  HTTP GET for a .dotm template right after a document is opened.

## How to spot it

- HTTP GET for a .dotm template
- Request fired when a document opens
- Template is a macro enabled ZIP (PK header)
- Payload lives in the remote template, not the document

## Questions

```
1. Technique?         Remote template injection
2. Server?             178.62.15.33
3. File?               /templates/report.dotm
4. Trigger?            document open
5. ATT&CK?             T1221
```

## How to solve it

1. Extract the evidence, password `hidden`: `7z x <sha256>.zip -phidden`. You get `remote-template-injection.pcap`.
2. Open `remote-template-injection.pcap` in Wireshark.
3. In Wireshark, paste this into the display filter bar and press Enter: `http.request.uri contains ".dotm"`  (the template fetch). Then Statistics > Conversations for the talkers.
4. Same filter from the shell: `tshark -r remote-template-injection.pcap -Y "http.request.uri contains ".dotm""`, and `tshark -r remote-template-injection.pcap -q -z conv,ip`.
5. Confirm against the answer: HTTP GET for a .dotm template

## MITRE ATT&CK

- T1221 Template Injection

## Tools

Wireshark, tshark, zeek, tcpdump.

## Signature

Forensic Sample Blueprint | qayssarayra.com
