# Modbus Dnp3 Ics

## Scenario

An unauthenticated client on the OT network sent Modbus TCP write commands to a PLC, changing coils and registers (including a stop), the kind of ICS manipulation that affects physical process.

Evidence: `modbus-dnp3-ics.pcap`  
Capture date: 2024-07-21

## Timeline

~13:05  Modbus write-coil and write-register commands to a PLC on TCP 502, with no authentication.

## How to spot it

- Modbus TCP traffic on port 502
- Write function codes (05/06/15/16) from an engineering host
- Commands to a PLC on the OT segment
- No authentication in the protocol

## Questions

```
1. Protocol?          Modbus TCP (502)
2. Function?           Write Coil / Write Register
3. Target?             PLC 10.10.3.10
4. Effect?             change/stop the process
5. Technique?          ICS command manipulation
```

## MITRE ATT&CK

- T0855 Unauthorized Command Message
- T0831 Manipulation of Control

## Tools

Wireshark, tshark, zeek, tcpdump.

## Signature

Forensic Sample Blueprint | qayssarayra.com
