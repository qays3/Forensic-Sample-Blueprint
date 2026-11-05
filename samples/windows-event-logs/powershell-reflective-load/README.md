# Powershell Reflective Load

## Scenario

An in memory .NET assembly load from a base64 blob (starts with the MZ header TVqQ), the fileless reflective loading pattern.

Host: WS-FIN-07  
Evidence: `powershell-operational.evtx` (real Windows PowerShell/Operational EVTX, one chunk)  
Channel: Microsoft-Windows-PowerShell/Operational

## Timeline

A 4104 block base64 decodes a PE and loads it with Reflection.Assembly.Load, never touching disk.

## How to spot it

- 4104 with FromBase64String and Reflection.Assembly.Load
- Base64 begins TVqQ (MZ header)
- No file written, loaded in memory
- Entry point invoked after load

## Questions

```
1. Log?               4104
2. Tell?               Base64 MZ then Assembly.Load
3. On disk?            No, reflective
4. Invokes?            the assembly entry point
5. Technique?          Reflective code loading
```

## How to solve it

1. Extract the evidence, password `hidden`: `7z x 224e78d7a1d096e840da31496836fbc0852d78d4e2368218b49cb69b556c134e.zip -phidden`. You get `powershell-operational.evtx`.
2. Open it in Event Viewer: Action > Open Saved Log, or parse it.
3. Pull the script blocks: `Get-WinEvent -Path powershell-operational.evtx -FilterXPath "*[System[EventID=4104]]" | Select-Object -ExpandProperty Message`, or `wevtutil qe powershell-operational.evtx /lf:true /q:"*[System[EventID=4104]]" /f:text`, or `EvtxECmd.exe -f powershell-operational.evtx --csv . --csvf out.csv`.
4. Read the ScriptBlockText and find the malicious one among the normal blocks.
5. Confirm against the answer: 4104 with FromBase64String and Reflection.Assembly.Load

## MITRE ATT&CK

- T1620 Reflective Code Loading
- T1059.001 PowerShell

## Tools

Event Viewer, Get-WinEvent, wevtutil, EvtxECmd, Chainsaw, Timeline Explorer.

## Signature

Forensic Sample Blueprint | qayssarayra.com
