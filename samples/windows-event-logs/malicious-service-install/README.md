# Malicious Service Install

## Scenario

A service is installed pointing at a binary in ProgramData, a common persistence and execution trick seen in 7045.

Host: DESKTOP-V96574F  
Evidence: `system.evtx` (real Windows EVTX, one chunk)  
Channel: System

## Timeline

A 7045 service install whose image path is a loose binary in ProgramData.

## How to spot it

- 7045 with an ImagePath outside System32
- Binary sits in ProgramData
- Generic service name hiding a payload
- New service, not a known product

## Questions

```
1. Log?               7045
2. ImagePath?           C:\ProgramData\svc.exe
3. Tell?               non system path
4. Goal?               persistence/execution
5. Technique?          new service
```

## How to solve it

1. Extract the evidence, password `hidden`: `7z x 1b4ef6d758b24b91be387f13de9503ae6a300dd0a10c05c208b18d71d3e2dc4d.zip -phidden`. You get `system.evtx`.
2. Open it in Event Viewer: Action > Open Saved Log, or parse it.
3. Filter to the event: `Get-WinEvent -Path system.evtx -FilterXPath "*[System[EventID={'7045': '7045', '4624': '4624', '4688': '4688'}]]"` , or `wevtutil qe system.evtx /lf:true /f:text`, or `EvtxECmd.exe -f system.evtx --csv . --csvf out.csv`.
4. Read the event data and find the malicious one among the normal records.
5. Confirm against the answer: 7045 with an ImagePath outside System32

## MITRE ATT&CK

- T1543.003 Windows Service

## Tools

Event Viewer, Get-WinEvent, wevtutil, EvtxECmd, Chainsaw, Timeline Explorer.

## Signature

Forensic Sample Blueprint | qayssarayra.com
