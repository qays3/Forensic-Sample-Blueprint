# Office Spawns Powershell

## Scenario

Word spawns PowerShell, the classic macro execution chain, visible as a 4688 with a WINWORD parent and a powershell child.

Host: DESKTOP-V96574F  
Evidence: `security.evtx` (real Windows EVTX, one chunk)  
Channel: Security

## Timeline

A 4688 where the parent is WINWORD.EXE and the new process is powershell.exe.

## How to spot it

- 4688 parent is an Office app
- Child is powershell.exe or cmd.exe
- Office should not spawn a shell
- Macro or exploit execution

## Questions

```
1. Log?               4688
2. Parent?             WINWORD.EXE
3. Child?              powershell.exe
4. Why bad?            Office spawning a shell
5. Technique?          Macro execution
```

## How to solve it

1. Extract the evidence, password `hidden`: `7z x a1cc0f563b08a435c2a716b140743aea504ead1d0c8d6e769faf00d8cc328176.zip -phidden`. You get `security.evtx`.
2. Open it in Event Viewer: Action > Open Saved Log, or parse it.
3. Filter to the event: `Get-WinEvent -Path security.evtx -FilterXPath "*[System[EventID={'7045': '7045', '4624': '4624', '4688': '4688'}]]"` , or `wevtutil qe security.evtx /lf:true /f:text`, or `EvtxECmd.exe -f security.evtx --csv . --csvf out.csv`.
4. Read the event data and find the malicious one among the normal records.
5. Confirm against the answer: 4688 parent is an Office app

## MITRE ATT&CK

- T1566.001 Spearphishing Attachment
- T1059.001 PowerShell

## Tools

Event Viewer, Get-WinEvent, wevtutil, EvtxECmd, Chainsaw, Timeline Explorer.

## Signature

Forensic Sample Blueprint | qayssarayra.com
