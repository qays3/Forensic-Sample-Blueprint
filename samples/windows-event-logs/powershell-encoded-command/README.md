# Powershell Encoded Command

## Scenario

A PowerShell encoded command decoded to a download-and-run one liner, logged in full by script block logging (4104) even though it ran with -EncodedCommand.

Host: WS-FIN-07  
Evidence: `powershell-operational.evtx` (real Windows PowerShell/Operational EVTX, one chunk)  
Channel: Microsoft-Windows-PowerShell/Operational

## Timeline

A hidden PowerShell ran with -EncodedCommand; 4104 recorded the decoded IEX download cradle.

## How to spot it

- 4104 script block with IEX and Net.WebClient.DownloadString
- Runs from an encoded command, so 4688 shows only -enc
- Pulls a remote script from a fake domain
- Hidden window style

## Questions

```
1. Log?               4104 (script block)
2. Decoded to?          IEX DownloadString cradle
3. IOC?                cdn-update-corp[.]xyz
4. Why 4104?            It logs the decoded block
5. Technique?          PowerShell + encoded command
```

## How to solve it

1. Extract the evidence, password `hidden`: `7z x dffe46d1a52bdd4d79b5d3d89b462d62aaecb437bbb3d0adf437aab07f87b10c.zip -phidden`. You get `powershell-operational.evtx`.
2. Open it in Event Viewer: Action > Open Saved Log, or parse it.
3. Pull the script blocks: `Get-WinEvent -Path powershell-operational.evtx -FilterXPath "*[System[EventID=4104]]" | Select-Object -ExpandProperty Message`, or `wevtutil qe powershell-operational.evtx /lf:true /q:"*[System[EventID=4104]]" /f:text`, or `EvtxECmd.exe -f powershell-operational.evtx --csv . --csvf out.csv`.
4. Read the ScriptBlockText and find the malicious one among the normal blocks.
5. Confirm against the answer: 4104 script block with IEX and Net.WebClient.DownloadString

## MITRE ATT&CK

- T1059.001 PowerShell
- T1027 Obfuscated Files or Information

## Tools

Event Viewer, Get-WinEvent, wevtutil, EvtxECmd, Chainsaw, Timeline Explorer.

## Signature

Forensic Sample Blueprint | qayssarayra.com
