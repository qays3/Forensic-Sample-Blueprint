# Wmiexec Dcerpc

## Scenario

Impacket wmiexec from 10.10.1.66 to 10.10.2.13: a DCERPC call to Win32_Process Create over WMI spawns a command and redirects output to ADMIN$.

Evidence: `wmiexec-dcerpc.pcap`  
Capture date: 2022-12-02

## Timeline

~11:01  DCERPC to the WMI endpoint, Win32_Process Create of a cmd, output to ADMIN$.

## How to spot it

- DCERPC to the WMI interface
- Win32_Process Create in the call
- Output redirected to ADMIN$
- Endpoint mapper then a dynamic port

## Questions

```
1. Tool?              Impacket wmiexec
2. Interface?           WMI Win32_Process
3. Method?             Create (command)
4. Output?             ADMIN$
5. Technique?          WMI remote exec
```

## How to solve it

1. Extract the evidence, password `hidden`: `7z x <sha256>.zip -phidden`. You get `wmiexec-dcerpc.pcap`.
2. Open `wmiexec-dcerpc.pcap` in Wireshark.
3. In Wireshark, paste this into the display filter bar and press Enter: `tcp.port==135 || dcerpc`  (the WMI DCERPC calls). Then Statistics > Conversations for the talkers.
4. Same filter from the shell: `tshark -r wmiexec-dcerpc.pcap -Y "tcp.port==135 || dcerpc"`, and `tshark -r wmiexec-dcerpc.pcap -q -z conv,ip`.
5. Confirm against the answer: DCERPC to the WMI interface

## MITRE ATT&CK

- T1047 WMI
- T1021 Remote Services

## Tools

Wireshark, tshark, zeek, tcpdump.

## Signature

Forensic Sample Blueprint | qayssarayra.com
