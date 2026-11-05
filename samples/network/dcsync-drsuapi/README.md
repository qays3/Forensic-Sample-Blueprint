# Dcsync Drsuapi

## Scenario

A DCSync from a non DC host 10.10.1.66 against the DC 10.10.2.10: the drsuapi replication interface (DsGetNCChanges) is used to pull account secrets, which only a DC should request.

Evidence: `dcsync-drsuapi.pcap`  
Capture date: 2022-11-06

## Timeline

~10:13  drsuapi DsGetNCChanges from a workstation, not another DC.

## How to spot it

- drsuapi DsGetNCChanges from a non DC host
- Replication request from a workstation
- Pulling account secrets over DRSUAPI
- A machine that is not a DC asking to replicate

## Questions

```
1. Interface?          drsuapi (DRSUAPI)
2. Call?               DsGetNCChanges
3. Source?             a non DC host
4. Tell?               replication from a workstation
5. ATT&CK?             T1003.006
```

## How to solve it

1. Extract the evidence, password `hidden`: `7z x <sha256>.zip -phidden`. You get `dcsync-drsuapi.pcap`.
2. Open `dcsync-drsuapi.pcap` in Wireshark.
3. In Wireshark, paste this into the display filter bar and press Enter: `dcerpc`  (the DsGetNCChanges request). Then Statistics > Conversations for the talkers.
4. Same filter from the shell: `tshark -r dcsync-drsuapi.pcap -Y "dcerpc"`, and `tshark -r dcsync-drsuapi.pcap -q -z conv,ip`.
5. Confirm against the answer: drsuapi DsGetNCChanges from a non DC host

## MITRE ATT&CK

- T1003.006 DCSync

## Tools

Wireshark, tshark, zeek, tcpdump.

## Signature

Forensic Sample Blueprint | qayssarayra.com
