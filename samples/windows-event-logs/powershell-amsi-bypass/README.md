# Powershell Amsi Bypass

## Scenario

The classic AmsiUtils amsiInitFailed reflection bypass, run in a PowerShell session to blind the Antimalware Scan Interface before loading tooling.

Host: WS-FIN-07  
Evidence: `powershell-operational.evtx` (real Windows PowerShell/Operational EVTX, one chunk)  
Channel: Microsoft-Windows-PowerShell/Operational

## Timeline

A 4104 block sets amsiInitFailed to true via reflection, disabling AMSI for the session.

## How to spot it

- 4104 referencing AmsiUtils and amsiInitFailed
- Reflection to a NonPublic Static field
- Run before other tooling in the same session
- No file on disk, in memory only

## Questions

```
1. Log?               4104
2. Target?             AmsiUtils.amsiInitFailed
3. Effect?             AMSI disabled for the session
4. Method?             reflection SetValue true
5. Technique?          Impair defenses
```

## How to solve it

1. Extract the evidence, password `hidden`: `7z x b5c9ded2a8a176d7155498dfcdfa786ad606868127ea59dd232262e2cb2a6fdb.zip -phidden`. You get `powershell-operational.evtx`.
2. Open it in Event Viewer: Action > Open Saved Log, or parse it.
3. Pull the script blocks: `Get-WinEvent -Path powershell-operational.evtx -FilterXPath "*[System[EventID=4104]]" | Select-Object -ExpandProperty Message`, or `wevtutil qe powershell-operational.evtx /lf:true /q:"*[System[EventID=4104]]" /f:text`, or `EvtxECmd.exe -f powershell-operational.evtx --csv . --csvf out.csv`.
4. Read the ScriptBlockText and find the malicious one among the normal blocks.
5. Confirm against the answer: 4104 referencing AmsiUtils and amsiInitFailed

## MITRE ATT&CK

- T1562.001 Disable or Modify Tools

## Tools

Event Viewer, Get-WinEvent, wevtutil, EvtxECmd, Chainsaw, Timeline Explorer.

## Signature

Forensic Sample Blueprint | qayssarayra.com
