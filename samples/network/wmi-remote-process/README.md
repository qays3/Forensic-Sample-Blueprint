# Wmi Remote Process

## Scenario

Remote process creation and query over WMI from 10.10.1.66 to 10.10.2.32: the IWbemServices interface is used to create a process and enumerate running ones.

Evidence: `wmi-remote-process.pcap`  
Capture date: 2022-04-03

## Timeline

~12:00  WMI IWbemServices ExecMethod to create a process, then a process list query.

## How to spot it

- DCERPC to the WMI IWbemServices interface
- ExecMethod Win32_Process Create
- A process list query in the same session
- Remote WMI activity from one host

## Questions

```
1. Interface?          WMI IWbemServices
2. Action?             Win32_Process Create
3. Also?               process enumeration
4. Source?             operator host
5. ATT&CK?             T1047
```

## How to solve it

1. Extract the evidence, password `hidden`: `7z x <sha256>.zip -phidden`. You get `wmi-remote-process.pcap`.
2. Open `wmi-remote-process.pcap` in Wireshark.
3. In Wireshark, paste this into the display filter bar and press Enter: `dcerpc || tcp.port==135`  (the WMI calls). Then Statistics > Conversations for the talkers.
4. Same filter from the shell: `tshark -r wmi-remote-process.pcap -Y "dcerpc || tcp.port==135"`, and `tshark -r wmi-remote-process.pcap -q -z conv,ip`.
5. Confirm against the answer: DCERPC to the WMI IWbemServices interface

## MITRE ATT&CK

- T1047 WMI
- T1057 Process Discovery

## Tools

Wireshark, tshark, zeek, tcpdump.

## Signature

Forensic Sample Blueprint | qayssarayra.com
