# Windows

This sequence is worth memorizing:

```
NT 4      → IIS 4
2000      → IIS 5
XP        → IIS 5.1
2003      → IIS 6
2008      → IIS 7
```

The easiest anchor:

> **Windows 2000 → IIS 5.0**
> 
> 
> **Windows 2003 → IIS 6.0**
> 

```prolog
Windows 2003
→ LM + NT hash

LM
→ old + weak

NT hash
→ MD4-based
→ used by NTLM

Local accounts
→ SAM

Domain Controller
→ NTDS.dit

Vista / Server 2008+
→ LM storage disabled by default
```