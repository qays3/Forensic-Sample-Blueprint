# Powershell Domain Recon Powerview

## Scenario

PowerView domain recon in one session: enumerate users, computers, Domain Admins, and where the current user is local admin.

Host: WS-FIN-07  
Evidence: `powershell-operational.evtx` (real Windows PowerShell/Operational EVTX, one chunk)  
Channel: Microsoft-Windows-PowerShell/Operational

## Timeline

A 4104 block imports PowerView and runs Get-Domain* and Find-LocalAdminAccess.

## How to spot it

- 4104 with Get-Domain cmdlets and Find-LocalAdminAccess
- PowerView module imported
- Enumerates users, computers, Domain Admins
- Fileless AD recon

## Questions

```
1. Log?               4104
2. Tool?               PowerView
3. Queries?            users, computers, Domain Admins
4. Also?               Find-LocalAdminAccess
5. Technique?          Account/domain discovery
```

## How to solve it

1. Extract the evidence, password `hidden`: `7z x e2c0d73d71296d574d85ae685b0226d9520c331f1ede9b2ec1392c2972f80126.zip -phidden`. You get `powershell-operational.evtx`.
2. Open it in Event Viewer: Action > Open Saved Log, or parse it.
3. Pull the script blocks: `Get-WinEvent -Path powershell-operational.evtx -FilterXPath "*[System[EventID=4104]]" | Select-Object -ExpandProperty Message`, or `wevtutil qe powershell-operational.evtx /lf:true /q:"*[System[EventID=4104]]" /f:text`, or `EvtxECmd.exe -f powershell-operational.evtx --csv . --csvf out.csv`.
4. Read the ScriptBlockText and find the malicious one among the normal blocks.
5. Confirm against the answer: 4104 with Get-Domain cmdlets and Find-LocalAdminAccess

## MITRE ATT&CK

- T1087.002 Domain Account
- T1059.001 PowerShell

## Tools

Event Viewer, Get-WinEvent, wevtutil, EvtxECmd, Chainsaw, Timeline Explorer.

## Signature

Forensic Sample Blueprint | qayssarayra.com
