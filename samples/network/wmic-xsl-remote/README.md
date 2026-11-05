# Wmic Xsl Remote

## Scenario

WMIC runs a remote XSL stylesheet containing script (the squiblytwo technique): an HTTP GET for an .xsl served externally.

Evidence: `wmic-xsl-remote.pcap`  
Capture date: 2024-11-12

## Timeline

~10:03  HTTP GET for an .xsl stylesheet, XML with embedded script returned.

## How to spot it

- HTTP GET for an .xsl file
- Stylesheet with an embedded eval or script
- Fetched by wmic.exe /format
- Signed binary proxy execution

## Questions

```
1. Technique?         WMIC XSL (squiblytwo)
2. Server?             51.15.44.90
3. File?               /evil.xsl
4. Runner?             wmic.exe
5. ATT&CK?             T1220
```

## How to solve it

1. Extract the evidence, password `hidden`: `7z x <sha256>.zip -phidden`. You get `wmic-xsl-remote.pcap`.
2. Open `wmic-xsl-remote.pcap` in Wireshark.
3. In Wireshark, paste this into the display filter bar and press Enter: `http.request.uri contains ".xsl"`  (the stylesheet fetch). Then Statistics > Conversations for the talkers.
4. Same filter from the shell: `tshark -r wmic-xsl-remote.pcap -Y "http.request.uri contains ".xsl""`, and `tshark -r wmic-xsl-remote.pcap -q -z conv,ip`.
5. Confirm against the answer: HTTP GET for an .xsl file

## MITRE ATT&CK

- T1220 XSL Script Processing

## Tools

Wireshark, tshark, zeek, tcpdump.

## Signature

Forensic Sample Blueprint | qayssarayra.com
