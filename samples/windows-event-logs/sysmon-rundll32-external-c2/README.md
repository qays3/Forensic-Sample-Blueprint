# Sysmon Rundll32 External C2

## Scenario

rundll32 connects out to an attacker host, a sign of a DLL beacon, caught by Sysmon id 3.

Host: DESKTOP-V96574F  
Evidence: `sysmon-operational.evtx` (real Windows Sysmon EVTX, one chunk)  
Channel: Microsoft-Windows-Sysmon/Operational

## Timeline

A Sysmon id 3 where rundll32.exe connects to a fake host.

## How to spot it

- Sysmon 3 with Image rundll32.exe
- DestinationHostname is attacker infrastructure
- rundll32 making network connections is unusual
- Loaded DLL beaconing out

## Questions

```
1. Log?               Sysmon 3
2. Process?            rundll32.exe
3. Destination?         files-corp-cdn.xyz
4. Tell?               lolbin network connection
5. Technique?          C2
```

## How to solve it

1. Extract the evidence, password `hidden`: `7z x 2078398c0ba15dc5463cd0d5186801a011d844a24831aace963ca2257a07f0ad.zip -phidden`. You get `sysmon-operational.evtx`.
2. Open it in Event Viewer: Action > Open Saved Log, or parse it.
3. Filter and read: `Get-WinEvent -Path sysmon-operational.evtx | Where-Object Id -in 1,3 | Format-List TimeCreated,Id,Message`, or `EvtxECmd.exe -f sysmon-operational.evtx --csv . --csvf out.csv`, or `wevtutil qe sysmon-operational.evtx /lf:true /f:text`.
4. Find the malicious record among the normal Sysmon events.
5. Confirm against the answer: Sysmon 3 with Image rundll32.exe

## MITRE ATT&CK

- T1071 Application Layer Protocol
- T1218.011 Rundll32

## Tools

Event Viewer, Get-WinEvent, EvtxECmd, Chainsaw, Sysmon View, Timeline Explorer.

## Signature

Forensic Sample Blueprint | qayssarayra.com
