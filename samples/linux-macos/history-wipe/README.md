# history-wipe

## Scenario
After the attack the intruder clears shell history to cover their tracks.

## Timeline
```
~16:00  HISTFILE unset, history -c, .bash_history removed
```

## How to spot it
- HISTFILE set to /dev/null or unset
- history -c and removal of .bash_history
- A suspiciously empty or truncated history
- Anti forensics after activity

## Questions
```
1. Technique?          Shell history wipe
2. Commands?           unset HISTFILE, history -c, rm history
3. Goal?               Cover tracks
4. Tell?               Empty/short history
5. Host?               websrv01
```

## MITRE ATT&CK
- T1070.003 Indicator Removal: Clear Command History

## Tools
Hand written Linux logs with a workday of normal activity as background

## Signature
Forensic Sample Blueprint | qayssarayra.com
