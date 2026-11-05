# Sysmon Powershell External C2

## Scenario

PowerShell makes an outbound connection to an attacker host, caught by Sysmon network connection (id 3).

Host: DESKTOP-V96574F  
Evidence: `sysmon-operational.evtx` (real Windows Sysmon EVTX, one chunk)  
Channel: Microsoft-Windows-Sysmon/Operational

## Timeline

A Sysmon id 3 where powershell.exe connects out to a fake C2 host.

## How to spot it

- Sysmon 3 with Image powershell.exe
- DestinationHostname is a fake C2 domain
- PowerShell should rarely make raw outbound connections
- Beacon or download

## Questions

```
1. Log?               Sysmon 3 (network connect)
2. Process?            powershell.exe
3. Destination?         cdn-update-corp.xyz
4. Why bad?            shell making external connection
5. Technique?          C2
```

## How to solve it

1. Extract the evidence, password `hidden`: `7z x eaa4ecebd5f2e4ef79d77d66e6b745e660065ef412deccb892b4d8caece36d26.zip -phidden`. You get `sysmon-operational.evtx`.
2. Open it in Event Viewer: Action > Open Saved Log, or parse it.
3. Filter and read: `Get-WinEvent -Path sysmon-operational.evtx | Where-Object Id -in 1,3 | Format-List TimeCreated,Id,Message`, or `EvtxECmd.exe -f sysmon-operational.evtx --csv . --csvf out.csv`, or `wevtutil qe sysmon-operational.evtx /lf:true /f:text`.
4. Find the malicious record among the normal Sysmon events.
5. Confirm against the answer: Sysmon 3 with Image powershell.exe

## MITRE ATT&CK

- T1071.001 Web Protocols
- T1059.001 PowerShell

## Tools

Event Viewer, Get-WinEvent, EvtxECmd, Chainsaw, Sysmon View, Timeline Explorer.

## Signature

Forensic Sample Blueprint | qayssarayra.com
