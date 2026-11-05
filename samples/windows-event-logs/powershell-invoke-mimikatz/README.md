# Powershell Invoke Mimikatz

## Scenario

The in memory Invoke-Mimikatz pattern: pull the script and run sekurlsa::logonpasswords to dump credentials, fully visible in 4104.

Host: WS-FIN-07  
Evidence: `powershell-operational.evtx` (real Windows PowerShell/Operational EVTX, one chunk)  
Channel: Microsoft-Windows-PowerShell/Operational

## Timeline

A 4104 block downloads Invoke-Mimikatz and runs sekurlsa::logonpasswords.

## How to spot it

- 4104 with Invoke-Mimikatz and sekurlsa::logonpasswords
- Remote script pulled with IEX
- Credential dump via reflective Mimikatz
- No mimikatz.exe on disk

## Questions

```
1. Log?               4104
2. Tool?               Invoke-Mimikatz (in memory)
3. Command?            sekurlsa::logonpasswords
4. IOC?                cdn-update-corp[.]xyz
5. Technique?          LSASS credential dump
```

## How to solve it

1. Extract the evidence, password `hidden`: `7z x 3bd50a191aad4f4b727d39aaec2ed3ca24c52e36c2c595c414675f7a3b5d625e.zip -phidden`. You get `powershell-operational.evtx`.
2. Open it in Event Viewer: Action > Open Saved Log, or parse it.
3. Pull the script blocks: `Get-WinEvent -Path powershell-operational.evtx -FilterXPath "*[System[EventID=4104]]" | Select-Object -ExpandProperty Message`, or `wevtutil qe powershell-operational.evtx /lf:true /q:"*[System[EventID=4104]]" /f:text`, or `EvtxECmd.exe -f powershell-operational.evtx --csv . --csvf out.csv`.
4. Read the ScriptBlockText and find the malicious one among the normal blocks.
5. Confirm against the answer: 4104 with Invoke-Mimikatz and sekurlsa::logonpasswords

## MITRE ATT&CK

- T1003.001 LSASS Memory
- T1059.001 PowerShell

## Tools

Event Viewer, Get-WinEvent, wevtutil, EvtxECmd, Chainsaw, Timeline Explorer.

## Signature

Forensic Sample Blueprint | qayssarayra.com
