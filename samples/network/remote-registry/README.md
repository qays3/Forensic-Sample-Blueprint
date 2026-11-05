# Remote Registry

## Scenario

Remote registry changes from 10.10.1.66 on 10.10.2.26: the winreg DCERPC interface over SMB opens HKLM and writes a Run key value for persistence.

Evidence: `remote-registry.pcap`  
Capture date: 2024-12-01

## Timeline

~10:10  winreg DCERPC over IPC$, OpenHKLM, RegCreateKey and RegSetValue on a Run key.

## How to spot it

- winreg over DCERPC
- OpenHKLM then RegSetValue
- A Run key written remotely
- Over the IPC$ named pipe

## Questions

```
1. Interface?          winreg
2. Calls?              RegCreateKey, RegSetValue
3. Target?             HKLM Run key
4. Transport?          DCERPC over SMB
5. ATT&CK?             T1112
```

## How to solve it

1. Extract the evidence, password `hidden`: `7z x <sha256>.zip -phidden`. You get `remote-registry.pcap`.
2. Open `remote-registry.pcap` in Wireshark.
3. In Wireshark, paste this into the display filter bar and press Enter: `dcerpc || tcp.port==445`  (the winreg calls). Then Statistics > Conversations for the talkers.
4. Same filter from the shell: `tshark -r remote-registry.pcap -Y "dcerpc || tcp.port==445"`, and `tshark -r remote-registry.pcap -q -z conv,ip`.
5. Confirm against the answer: winreg over DCERPC

## MITRE ATT&CK

- T1112 Modify Registry
- T1021.002 SMB/Admin Shares

## Tools

Wireshark, tshark, zeek, tcpdump.

## Signature

Forensic Sample Blueprint | qayssarayra.com
