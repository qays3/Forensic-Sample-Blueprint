# Impacket Dcomexec

## Scenario

Impacket dcomexec from 10.10.1.66 to 10.10.2.22: a DCOM object (MMC20.Application) is invoked over DCERPC to run a command on the target.

Evidence: `impacket-dcomexec.pcap`  
Capture date: 2023-06-13

## Timeline

~11:01  Endpoint mapper lookup, then a DCOM ExecuteShellCommand over the dynamic port.

## How to spot it

- DCERPC to the endpoint mapper then a dynamic port
- A DCOM object method invoked (MMC20.Application)
- ExecuteShellCommand in the call
- Remote command execution over DCOM

## Questions

```
1. Tool?              Impacket dcomexec
2. Object?             MMC20.Application
3. Transport?          DCERPC/DCOM
4. Method?             ExecuteShellCommand
5. ATT&CK?             T1021.003
```

## How to solve it

1. Extract the evidence, password `hidden`: `7z x <sha256>.zip -phidden`. You get `impacket-dcomexec.pcap`.
2. Open `impacket-dcomexec.pcap` in Wireshark.
3. In Wireshark, paste this into the display filter bar and press Enter: `dcerpc || tcp.port==135`  (the DCOM calls). Then Statistics > Conversations for the talkers.
4. Same filter from the shell: `tshark -r impacket-dcomexec.pcap -Y "dcerpc || tcp.port==135"`, and `tshark -r impacket-dcomexec.pcap -q -z conv,ip`.
5. Confirm against the answer: DCERPC to the endpoint mapper then a dynamic port

## MITRE ATT&CK

- T1021.003 Distributed Component Object Model

## Tools

Wireshark, tshark, zeek, tcpdump.

## Signature

Forensic Sample Blueprint | qayssarayra.com
