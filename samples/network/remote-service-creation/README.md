# Remote Service Creation

## Scenario

A service is created remotely from 10.10.1.66 on 10.10.2.25 (sc \\host create): the svcctl DCERPC interface opens the SCM and creates then starts a service.

Evidence: `remote-service-creation.pcap`  
Capture date: 2024-03-12

## Timeline

~11:00  svcctl DCERPC over IPC$, OpenSCManager, CreateServiceW then StartServiceW.

## How to spot it

- svcctl over DCERPC
- OpenSCManager and CreateServiceW calls
- A new service with a binary path
- Started remotely

## Questions

```
1. Interface?          svcctl
2. Calls?              CreateServiceW, StartServiceW
3. Transport?          DCERPC over SMB
4. Source?             operator host
5. ATT&CK?             T1543.003
```

## How to solve it

1. Extract the evidence, password `hidden`: `7z x <sha256>.zip -phidden`. You get `remote-service-creation.pcap`.
2. Open `remote-service-creation.pcap` in Wireshark.
3. In Wireshark, paste this into the display filter bar and press Enter: `dcerpc || tcp.port==445`  (the svcctl calls). Then Statistics > Conversations for the talkers.
4. Same filter from the shell: `tshark -r remote-service-creation.pcap -Y "dcerpc || tcp.port==445"`, and `tshark -r remote-service-creation.pcap -q -z conv,ip`.
5. Confirm against the answer: svcctl over DCERPC

## MITRE ATT&CK

- T1543.003 Windows Service
- T1021.002 SMB/Admin Shares

## Tools

Wireshark, tshark, zeek, tcpdump.

## Signature

Forensic Sample Blueprint | qayssarayra.com
