# Meterpreter 4444

## Scenario

The victim connects to 185.220.101.9 on port 4444 and pulls a reflective DLL stage, the Metasploit Meterpreter default pattern.

Evidence: `meterpreter-4444.pcap`  
Capture date: 2021-02-11

## Timeline

~11:01  TCP 4444 connection to 185.220.101.9, a stager GET, then a large MZ payload.

## How to spot it

- Connection to the Metasploit default port 4444
- A stage download starting with MZ
- Reflective DLL in the response body
- Non standard port for HTTP

## Questions

```
1. C2?                185.220.101.9:4444
2. Port?               4444 (Metasploit default)
3. Stage?              reflective DLL (MZ)
4. Tool?               Meterpreter
5. Technique?          C2
```

## How to solve it

1. Extract the evidence, password `hidden`: `7z x <sha256>.zip -phidden`. You get `meterpreter-4444.pcap`.
2. Open `meterpreter-4444.pcap` in Wireshark.
3. In Wireshark, paste this into the display filter bar and press Enter: `tcp.port==4444`  (Meterpreter stager on 4444). Then Statistics > Conversations for the talkers.
4. Same filter from the shell: `tshark -r meterpreter-4444.pcap -Y "tcp.port==4444"`, and `tshark -r meterpreter-4444.pcap -q -z conv,ip`.
5. Confirm against the answer: Connection to the Metasploit default port 4444

## MITRE ATT&CK

- T1071 Application Layer Protocol
- T1055 Process Injection

## Tools

Wireshark, tshark, zeek, tcpdump.

## Signature

Forensic Sample Blueprint | qayssarayra.com
