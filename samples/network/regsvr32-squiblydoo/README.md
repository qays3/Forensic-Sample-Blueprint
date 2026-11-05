# Regsvr32 Squiblydoo

## Scenario

The Squiblydoo technique: regsvr32 fetches a remote .sct scriptlet over HTTP and runs its embedded script, a signed binary proxy execution.

Evidence: `regsvr32-squiblydoo.pcap`  
Capture date: 2019-11-01

## Timeline

~10:01  HTTP GET for a .sct scriptlet, XML with embedded script returned.

## How to spot it

- HTTP GET for a .sct scriptlet
- XML scriptlet with embedded script
- Fetched by regsvr32.exe
- Signed binary proxy execution

## Questions

```
1. Technique?         Squiblydoo
2. Server?             193.37.212.44
3. File?               /x.sct
4. Runner?             regsvr32.exe
5. ATT&CK?             T1218.010
```

## How to solve it

1. Extract the evidence, password `hidden`: `7z x <sha256>.zip -phidden`. You get `regsvr32-squiblydoo.pcap`.
2. Open `regsvr32-squiblydoo.pcap` in Wireshark.
3. In Wireshark, paste this into the display filter bar and press Enter: `http.request.uri contains ".sct"`  (the scriptlet fetch). Then Statistics > Conversations for the talkers.
4. Same filter from the shell: `tshark -r regsvr32-squiblydoo.pcap -Y "http.request.uri contains ".sct""`, and `tshark -r regsvr32-squiblydoo.pcap -q -z conv,ip`.
5. Confirm against the answer: HTTP GET for a .sct scriptlet

## MITRE ATT&CK

- T1218.010 Regsvr32

## Tools

Wireshark, tshark, zeek, tcpdump.

## Signature

Forensic Sample Blueprint | qayssarayra.com
