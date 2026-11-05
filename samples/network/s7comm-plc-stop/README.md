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

## MITRE ATT&CK

- T0855 Unauthorized Command Message
- T0816 Device Restart/Shutdown

## Tools

Wireshark, tshark, zeek, tcpdump.

## Signature

Forensic Sample Blueprint | qayssarayra.com
