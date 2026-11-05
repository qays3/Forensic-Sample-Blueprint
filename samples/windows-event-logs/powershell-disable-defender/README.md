# Powershell Disable Defender

## Scenario

Defender is weakened from PowerShell: real time monitoring off and an exclusion path added, captured by 4104.

Host: WS-FIN-07  
Evidence: `powershell-operational.evtx` (real Windows PowerShell/Operational EVTX, one chunk)  
Channel: Microsoft-Windows-PowerShell/Operational

## Timeline

A 4104 block turns off real time monitoring and adds a ProgramData exclusion.

## How to spot it

- 4104 with Set-MpPreference DisableRealtimeMonitoring true
- Add-MpPreference ExclusionPath
- Done before dropping payloads
- Defender weakened, not uninstalled

## Questions

```
1. Log?               4104
2. Cmdlets?            Set-MpPreference, Add-MpPreference
3. Effect?             RTP off, folder excluded
4. Excluded?           C:\ProgramData
5. Technique?          Impair defenses
```

## How to solve it

1. Extract the evidence, password `hidden`: `7z x 48865df5567a64953adb500b783e28431add8f0051371b6e09809a9ea265fb16.zip -phidden`. You get `powershell-operational.evtx`.
2. Open it in Event Viewer: Action > Open Saved Log, or parse it.
3. Pull the script blocks: `Get-WinEvent -Path powershell-operational.evtx -FilterXPath "*[System[EventID=4104]]" | Select-Object -ExpandProperty Message`, or `wevtutil qe powershell-operational.evtx /lf:true /q:"*[System[EventID=4104]]" /f:text`, or `EvtxECmd.exe -f powershell-operational.evtx --csv . --csvf out.csv`.
4. Read the ScriptBlockText and find the malicious one among the normal blocks.
5. Confirm against the answer: 4104 with Set-MpPreference DisableRealtimeMonitoring true

## MITRE ATT&CK

- T1562.001 Disable or Modify Tools

## Tools

Event Viewer, Get-WinEvent, wevtutil, EvtxECmd, Chainsaw, Timeline Explorer.

## Signature

Forensic Sample Blueprint | qayssarayra.com
