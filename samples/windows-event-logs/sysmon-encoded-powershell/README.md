# Sysmon Encoded Powershell

## Scenario

PowerShell runs with an encoded command, visible in Sysmon id 1 with the -enc command line.

Host: DESKTOP-V96574F  
Evidence: `sysmon-operational.evtx` (real Windows Sysmon EVTX, one chunk)  
Channel: Microsoft-Windows-Sysmon/Operational

## Timeline

A Sysmon id 1 for powershell.exe with -w hidden -enc.

## How to spot it

- Sysmon 1 powershell.exe with -enc
- Hidden window style
- Encoded payload on the command line
- Often follows a document or lolbin

## Questions

```
1. Log?               Sysmon 1
2. Binary?             powershell.exe
3. CommandLine?         -nop -w hidden -enc <base64>
4. Tell?               hidden + encoded
5. Technique?          PowerShell execution
```

## How to solve it

1. Extract the evidence, password `hidden`: `7z x d755235e0516e43e8fc8892e3ea5a393e123e0516d006a65cc1fada02d4005eb.zip -phidden`. You get `sysmon-operational.evtx`.
2. Open it in Event Viewer: Action > Open Saved Log, or parse it.
3. Filter and read: `Get-WinEvent -Path sysmon-operational.evtx | Where-Object Id -in 1,3 | Format-List TimeCreated,Id,Message`, or `EvtxECmd.exe -f sysmon-operational.evtx --csv . --csvf out.csv`, or `wevtutil qe sysmon-operational.evtx /lf:true /f:text`.
4. Find the malicious record among the normal Sysmon events.
5. Confirm against the answer: Sysmon 1 powershell.exe with -enc

## MITRE ATT&CK

- T1059.001 PowerShell
- T1027 Obfuscated Files or Information

## Tools

Event Viewer, Get-WinEvent, EvtxECmd, Chainsaw, Sysmon View, Timeline Explorer.

## Signature

Forensic Sample Blueprint | qayssarayra.com
