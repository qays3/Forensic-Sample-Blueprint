# Masquerading Svchost User Path

## Scenario

A process named svchost.exe runs from a user writable path instead of System32, a masquerade caught in 4688.

Host: DESKTOP-V96574F  
Evidence: `security.evtx` (real Windows EVTX, one chunk)  
Channel: Security

## Timeline

A 4688 where NewProcessName is svchost.exe under C:\Users\Public.

## How to spot it

- 4688 with a system name (svchost) in a non system path
- svchost should only run from System32
- User writable location
- Masquerading to blend in

## Questions

```
1. Log?               4688
2. Image?              svchost.exe
3. Path?               C:\Users\Public (wrong)
4. Tell?               system name, user path
5. Technique?          Masquerading
```

## How to solve it

1. Extract the evidence, password `hidden`: `7z x 72a26281e94a23602f13d1af61fb4adf779309cccef17ff2720a879458f0065d.zip -phidden`. You get `security.evtx`.
2. Open it in Event Viewer: Action > Open Saved Log, or parse it.
3. Filter to the event: `Get-WinEvent -Path security.evtx -FilterXPath "*[System[EventID={'7045': '7045', '4624': '4624', '4688': '4688'}]]"` , or `wevtutil qe security.evtx /lf:true /f:text`, or `EvtxECmd.exe -f security.evtx --csv . --csvf out.csv`.
4. Read the event data and find the malicious one among the normal records.
5. Confirm against the answer: 4688 with a system name (svchost) in a non system path

## MITRE ATT&CK

- T1036.005 Match Legitimate Name or Location

## Tools

Event Viewer, Get-WinEvent, wevtutil, EvtxECmd, Chainsaw, Timeline Explorer.

## Signature

Forensic Sample Blueprint | qayssarayra.com
