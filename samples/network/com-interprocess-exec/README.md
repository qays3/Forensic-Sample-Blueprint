# Com Interprocess Exec

## Scenario

Inter process execution via COM from 10.10.1.66 to 10.10.2.31: a DCOM object (ShellBrowserWindow) is driven over DCERPC to launch a command.

Evidence: `com-interprocess-exec.pcap`  
Capture date: 2020-09-09

## Timeline

~11:01  Endpoint mapper lookup, then a ShellBrowserWindow DCOM method to run a command.

## How to spot it

- DCERPC to a DCOM object
- ShellBrowserWindow or MMC20 in the call
- A ShellExecute or Navigate method
- Execution brokered through COM

## Questions

```
1. Technique?         COM execution
2. Object?             ShellBrowserWindow
3. Transport?          DCERPC/DCOM
4. Method?             ShellExecute
5. ATT&CK?             T1559.001
```

## How to solve it

1. Extract the evidence, password `hidden`: `7z x <sha256>.zip -phidden`. You get `com-interprocess-exec.pcap`.
2. Open `com-interprocess-exec.pcap` in Wireshark.
3. In Wireshark, paste this into the display filter bar and press Enter: `dcerpc`  (the DCOM method call). Then Statistics > Conversations for the talkers.
4. Same filter from the shell: `tshark -r com-interprocess-exec.pcap -Y "dcerpc"`, and `tshark -r com-interprocess-exec.pcap -q -z conv,ip`.
5. Confirm against the answer: DCERPC to a DCOM object

## MITRE ATT&CK

- T1559.001 Component Object Model

## Tools

Wireshark, tshark, zeek, tcpdump.

## Signature

Forensic Sample Blueprint | qayssarayra.com
