# Dcom Lateral

## Scenario

Lateral movement over DCOM from 10.10.1.66 to 10.10.2.23: the ShellWindows object is used to launch a process remotely via ShellExecute.

Evidence: `dcom-lateral.pcap`  
Capture date: 2021-07-13

## Timeline

~10:06  Endpoint mapper lookup, then ShellWindows ShellExecute over the dynamic port.

## How to spot it

- DCERPC to a DCOM object
- ShellWindows or ShellBrowserWindow in the call
- ShellExecute invoked remotely
- Lateral movement without a service or scheduled task

## Questions

```
1. Object?             ShellWindows
2. Transport?          DCERPC/DCOM
3. Method?             ShellExecute
4. Source?             operator host
5. ATT&CK?             T1021.003
```

## How to solve it

1. Extract the evidence, password `hidden`: `7z x <sha256>.zip -phidden`. You get `dcom-lateral.pcap`.
2. Open `dcom-lateral.pcap` in Wireshark.
3. In Wireshark, paste this into the display filter bar and press Enter: `dcerpc`  (the DCOM method call). Then Statistics > Conversations for the talkers.
4. Same filter from the shell: `tshark -r dcom-lateral.pcap -Y "dcerpc"`, and `tshark -r dcom-lateral.pcap -q -z conv,ip`.
5. Confirm against the answer: DCERPC to a DCOM object

## MITRE ATT&CK

- T1021.003 Distributed Component Object Model

## Tools

Wireshark, tshark, zeek, tcpdump.

## Signature

Forensic Sample Blueprint | qayssarayra.com
