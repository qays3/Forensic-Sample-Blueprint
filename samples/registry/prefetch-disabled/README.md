# Prefetch Disabled

## Scenario

Prefetch is disabled by setting EnablePrefetcher to 0, removing a record of what executables have run.

Host: WS-FIN-07  
Evidence: `registry.reg` (exported registry keys)  
Approx hive last write: 2019-01-07 18:02

## Timeline

EnablePrefetcher set to 0 under PrefetchParameters.

## How to spot it

- EnablePrefetcher set to 0 (normally 3)
- Stops new .pf files being written
- Anti forensics against execution evidence
- A single, deliberate value change

## Questions

```
1. Value?             EnablePrefetcher
2. Set to?             0
3. Normal?             3
4. Effect?             No Prefetch execution record
5. Technique?          Impair defenses / indicator removal
```

## MITRE ATT&CK

- T1562.001 Disable or Modify Tools
- T1070 Indicator Removal

## Tools

Registry Explorer, regedit, RECmd, reg query. Open the .reg in a text editor to read the keys directly.

## Signature

Forensic Sample Blueprint | qayssarayra.com
