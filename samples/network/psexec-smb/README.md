# Psexec Smb

## Scenario

PsExec over SMB from 10.10.1.66 to 10.10.2.12: an NTLM session to ADMIN$, a write of PSEXESVC.exe, a service create over svcctl, then a named pipe for input and output.

Evidence: `psexec-smb.pcap`  
Capture date: 2023-02-04

## Timeline

~12:00  SMB session to ADMIN$, PSEXESVC.exe written, service created over svcctl, pipe opened.

## How to spot it

- SMB write of PSEXESVC.exe to ADMIN$
- Service create over the svcctl pipe
- A pipe named PSEXESVC for input and output
- NTLM auth from the operator host

## Questions

```
1. Tool?              PsExec
2. Share?              ADMIN$
3. Service?            PSEXESVC via svcctl
4. Pipe?               PSEXESVC
5. Technique?          Remote service exec
```

## How to solve it

1. Extract the evidence, password `hidden`: `7z x <sha256>.zip -phidden`. You get `psexec-smb.pcap`.
2. Open `psexec-smb.pcap` in Wireshark.
3. In Wireshark, paste this into the display filter bar and press Enter: `tcp.port==445`  (the SMB admin share and svcctl activity). Then Statistics > Conversations for the talkers.
4. Same filter from the shell: `tshark -r psexec-smb.pcap -Y "tcp.port==445"`, and `tshark -r psexec-smb.pcap -q -z conv,ip`.
5. Confirm against the answer: SMB write of PSEXESVC.exe to ADMIN$

## MITRE ATT&CK

- T1021.002 SMB/Admin Shares
- T1569.002 Service Execution

## Tools

Wireshark, tshark, zeek, tcpdump.

## Signature

Forensic Sample Blueprint | qayssarayra.com
