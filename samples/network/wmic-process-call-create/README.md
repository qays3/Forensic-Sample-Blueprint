# Wmic Process Call Create

## Scenario

Remote execution with wmic process call create from 10.10.1.66 to 10.10.2.30: a DCERPC call to Win32_Process Create spawns a process on the target.

Evidence: `wmic-process-call-create.pcap`  
Capture date: 2023-02-23

## Timeline

~10:03  Endpoint mapper lookup then a Win32_Process Create over WMI.

## How to spot it

- DCERPC to the WMI interface
- Win32_Process Create in the call
- A ProcessId returned
- wmic /node process call create pattern

## Questions

```
1. Tool?              wmic process call create
2. Interface?           WMI Win32_Process
3. Method?             Create
4. Result?             new ProcessId
5. ATT&CK?             T1047
```

## How to solve it

1. Extract the evidence, password `hidden`: `7z x <sha256>.zip -phidden`. You get `wmic-process-call-create.pcap`.
2. Open `wmic-process-call-create.pcap` in Wireshark.
3. In Wireshark, paste this into the display filter bar and press Enter: `dcerpc || tcp.port==135`  (the WMI Create call). Then Statistics > Conversations for the talkers.
4. Same filter from the shell: `tshark -r wmic-process-call-create.pcap -Y "dcerpc || tcp.port==135"`, and `tshark -r wmic-process-call-create.pcap -q -z conv,ip`.
5. Confirm against the answer: DCERPC to the WMI interface

## MITRE ATT&CK

- T1047 WMI

## Tools

Wireshark, tshark, zeek, tcpdump.

## Signature

Forensic Sample Blueprint | qayssarayra.com
