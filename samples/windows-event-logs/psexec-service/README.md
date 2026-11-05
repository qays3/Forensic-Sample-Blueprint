# Psexec Service

## Scenario

Sysinternals PsExec drops the PSEXESVC service. A 7045 shows the service install with the PSEXESVC image path.

Host: DESKTOP-V96574F  
Evidence: `system.evtx` (real Windows EVTX, one chunk)  
Channel: System

## Timeline

A 7045 service install for PSEXESVC running from the Windows root.

## How to spot it

- 7045 ServiceName PSEXESVC
- ImagePath is PSEXESVC.exe in the Windows root
- Service install by remote admin tooling
- Pairs with a type 3 logon from the operator

## Questions

```
1. Log?               7045 (service installed)
2. Service?            PSEXESVC
3. Tool?               PsExec
4. Remote?             yes, from the operator host
5. Technique?          Service execution
```

## How to solve it

1. Extract the evidence, password `hidden`: `7z x e4f65232446b9f44ec1d789fdd48f126f070905d0da3bef93f6368ba909396ad.zip -phidden`. You get `system.evtx`.
2. Open it in Event Viewer: Action > Open Saved Log, or parse it.
3. Filter to the event: `Get-WinEvent -Path system.evtx -FilterXPath "*[System[EventID={'7045': '7045', '4624': '4624', '4688': '4688'}]]"` , or `wevtutil qe system.evtx /lf:true /f:text`, or `EvtxECmd.exe -f system.evtx --csv . --csvf out.csv`.
4. Read the event data and find the malicious one among the normal records.
5. Confirm against the answer: 7045 ServiceName PSEXESVC

## MITRE ATT&CK

- T1569.002 Service Execution
- T1021.002 SMB/Admin Shares

## Tools

Event Viewer, Get-WinEvent, wevtutil, EvtxECmd, Chainsaw, Timeline Explorer.

## Signature

Forensic Sample Blueprint | qayssarayra.com
