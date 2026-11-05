# Winrm Remoting Traffic

## Scenario

PowerShell remoting over WinRM from 10.10.1.66 to 10.10.2.14: SOAP over HTTP on 5985 creates a remote shell and runs commands.

Evidence: `winrm-remoting-traffic.pcap`  
Capture date: 2022-11-23

## Timeline

~13:00  WSMan POST to /wsman on 5985, shell create then CommandLine execution.

## How to spot it

- HTTP POST to /wsman on port 5985
- SOAP with rsp:Shell and rsp:CommandLine
- Negotiate or Kerberos auth
- Remote command output in the stream

## Questions

```
1. Protocol?          WinRM (WSMan) 5985
2. Operation?           Shell + CommandLine
3. Command?            powershell whoami
4. Tool?               PS Remoting
5. Technique?          WinRM remote exec
```

## How to solve it

1. Extract the evidence, password `hidden`: `7z x <sha256>.zip -phidden`. You get `winrm-remoting-traffic.pcap`.
2. Open `winrm-remoting-traffic.pcap` in Wireshark.
3. In Wireshark, paste this into the display filter bar and press Enter: `tcp.port==5985`  (the WSMan traffic). Then Statistics > Conversations for the talkers.
4. Same filter from the shell: `tshark -r winrm-remoting-traffic.pcap -Y "tcp.port==5985"`, and `tshark -r winrm-remoting-traffic.pcap -q -z conv,ip`.
5. Confirm against the answer: HTTP POST to /wsman on port 5985

## MITRE ATT&CK

- T1021.006 Windows Remote Management

## Tools

Wireshark, tshark, zeek, tcpdump.

## Signature

Forensic Sample Blueprint | qayssarayra.com
