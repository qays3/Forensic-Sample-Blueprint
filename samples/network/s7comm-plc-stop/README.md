# S7Comm Plc Stop

## Scenario

An attacker spoke S7comm to a Siemens PLC on TCP 102 and issued a PLC STOP, halting the controller and the process it runs.

Evidence: `s7comm-plc-stop.pcap`  
Capture date: 2023-02-12

## Timeline

~11:10  COTP/S7 session setup to a PLC, then an S7 STOP function is sent.

## How to spot it

- S7comm traffic on TCP 102 (ISO-TSAP)
- COTP connect then S7 setup-communication
- A PLC STOP function code
- From an unexpected host, no auth

## Questions

```
1. Protocol?          S7comm (TCP 102)
2. Target?             Siemens PLC 10.10.3.11
3. Command?            PLC STOP
4. Effect?             halt the controller
5. Technique?          ICS stop command
```

## How to solve it

1. Extract the evidence, password `hidden`: `7z x 13c9725629530fd25a7b04ad53503ae9c7bbcb6d9a36874c15c0aab095c037f6.zip -phidden`. You get `s7comm-plc-stop.pcap`.
2. Open `s7comm-plc-stop.pcap` in Wireshark.
3. In Wireshark, paste this into the display filter bar and press Enter: `tcp.port==102`  (S7comm/COTP on tcp.port==102). Then Statistics > Conversations to see the talkers.
4. Same filter from the shell with tshark: `tshark -r s7comm-plc-stop.pcap -Y "tcp.port==102"`, and list talkers with `tshark -r s7comm-plc-stop.pcap -q -z conv,ip`.
5. Confirm against the answer: S7comm traffic on TCP 102 (ISO-TSAP)

## MITRE ATT&CK

- T0855 Unauthorized Command Message
- T0816 Device Restart/Shutdown

## Tools

Wireshark, tshark, zeek, tcpdump.

## Signature

Forensic Sample Blueprint | qayssarayra.com
