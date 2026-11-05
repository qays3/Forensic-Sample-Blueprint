# Impacket Smbexec

## Scenario

Impacket smbexec from 10.10.1.66 to 10.10.2.21: a temporary service is created over svcctl to run each command, output written to a share and read back, a semi interactive shell over SMB.

Evidence: `impacket-smbexec.pcap`  
Capture date: 2025-11-10

## Timeline

~10:00  SMB to IPC$, a short lived service per command over svcctl, output read from the share.

## How to spot it

- A service created and deleted per command
- svcctl over the IPC$ pipe
- Command output written to and read from a share
- NTLM auth from the operator host

## Questions

```
1. Tool?              Impacket smbexec
2. Pipe?               svcctl over IPC$
3. Pattern?            service per command
4. Output?             share file
5. Technique?          Remote service exec
```

## How to solve it

1. Extract the evidence, password `hidden`: `7z x <sha256>.zip -phidden`. You get `impacket-smbexec.pcap`.
2. Open `impacket-smbexec.pcap` in Wireshark.
3. In Wireshark, paste this into the display filter bar and press Enter: `tcp.port==445`  (the per command service activity). Then Statistics > Conversations for the talkers.
4. Same filter from the shell: `tshark -r impacket-smbexec.pcap -Y "tcp.port==445"`, and `tshark -r impacket-smbexec.pcap -q -z conv,ip`.
5. Confirm against the answer: A service created and deleted per command

## MITRE ATT&CK

- T1021.002 SMB/Admin Shares
- T1569.002 Service Execution

## Tools

Wireshark, tshark, zeek, tcpdump.

## Signature

Forensic Sample Blueprint | qayssarayra.com
