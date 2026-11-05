# Powershell Download Cradle

## Scenario

A download cradle that saves a payload to the temp folder and runs it, captured by script block logging.

Host: WS-FIN-07  
Evidence: `powershell-operational.evtx` (real Windows PowerShell/Operational EVTX, one chunk)  
Channel: Microsoft-Windows-PowerShell/Operational

## Timeline

A 4104 block downloads s.exe to %TEMP% and starts it.

## How to spot it

- 4104 with DownloadFile to %TEMP%
- Start-Process of the downloaded binary
- Fake delivery domain
- Fetch then execute in one block

## Questions

```
1. Log?               4104
2. Action?             DownloadFile + Start-Process
3. Drop path?           %TEMP%\s.exe
4. IOC?                files-corp-cdn[.]xyz
5. Technique?          Ingress tool transfer
```

## How to solve it

1. Extract the evidence, password `hidden`: `7z x 1376d98f7f78486302ee491a895ef22e6dfcb99c9a8d1c9dadbea3b05d69d685.zip -phidden`. You get `powershell-operational.evtx`.
2. Open it in Event Viewer: Action > Open Saved Log, or parse it.
3. Pull the script blocks: `Get-WinEvent -Path powershell-operational.evtx -FilterXPath "*[System[EventID=4104]]" | Select-Object -ExpandProperty Message`, or `wevtutil qe powershell-operational.evtx /lf:true /q:"*[System[EventID=4104]]" /f:text`, or `EvtxECmd.exe -f powershell-operational.evtx --csv . --csvf out.csv`.
4. Read the ScriptBlockText and find the malicious one among the normal blocks.
5. Confirm against the answer: 4104 with DownloadFile to %TEMP%

## MITRE ATT&CK

- T1105 Ingress Tool Transfer
- T1059.001 PowerShell

## Tools

Event Viewer, Get-WinEvent, wevtutil, EvtxECmd, Chainsaw, Timeline Explorer.

## Signature

Forensic Sample Blueprint | qayssarayra.com
