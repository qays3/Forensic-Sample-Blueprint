# Powershell Reverse Shell

## Scenario

A PowerShell TCP reverse shell to an external host on 4444, the full loop captured by script block logging.

Host: WS-FIN-07  
Evidence: `powershell-operational.evtx` (real Windows PowerShell/Operational EVTX, one chunk)  
Channel: Microsoft-Windows-PowerShell/Operational

## Timeline

A 4104 block opens a TCPClient to an external IP on 4444 and runs received commands with iex.

## How to spot it

- 4104 with Net.Sockets.TCPClient to an external IP
- Port 4444
- iex of data read from the socket
- Classic PowerShell reverse shell loop

## Questions

```
1. Log?               4104
2. Connects to?         185.220.101.9:4444
3. Loop?               read, iex, write back
4. Shell type?          PowerShell reverse shell
5. Technique?          Command and control
```

## How to solve it

1. Extract the evidence, password `hidden`: `7z x ce850f47c92635ede996f5effdc41d6145d202986509df44cd3bb6a09a109bf9.zip -phidden`. You get `powershell-operational.evtx`.
2. Open it in Event Viewer: Action > Open Saved Log, or parse it.
3. Pull the script blocks: `Get-WinEvent -Path powershell-operational.evtx -FilterXPath "*[System[EventID=4104]]" | Select-Object -ExpandProperty Message`, or `wevtutil qe powershell-operational.evtx /lf:true /q:"*[System[EventID=4104]]" /f:text`, or `EvtxECmd.exe -f powershell-operational.evtx --csv . --csvf out.csv`.
4. Read the ScriptBlockText and find the malicious one among the normal blocks.
5. Confirm against the answer: 4104 with Net.Sockets.TCPClient to an external IP

## MITRE ATT&CK

- T1059.001 PowerShell
- T1571 Non-Standard Port

## Tools

Event Viewer, Get-WinEvent, wevtutil, EvtxECmd, Chainsaw, Timeline Explorer.

## Signature

Forensic Sample Blueprint | qayssarayra.com
