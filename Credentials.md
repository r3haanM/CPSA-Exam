# Credentials

```
Q: default password for the $user on Oracle 9i
SYS
↓
CHANGE_ON_INSTALL

SYSTEM
↓
MANAGER

DBSNMP
↓
DBSNMP

SCOTT
↓
TIGER
```

Memorize this:

```
Cisco password 7
→ Type 7
→ reversible
→ weak obfuscation
→ Vigenère-style
```

A very useful Cisco password-type cheat sheet is:

```
Type 0 → plaintext
Type 5 → MD5-based hash
Type 7 → reversible weak encoding ⭐
Type 8 → PBKDF2-SHA256
Type 9 → scrypt
```

### Databases

Very useful mental model:

```nix
Standalone/member Windows machine
        ↓
SAM
        ↓
local accounts

Domain Controller
        ↓
NTDS.dit
        ↓
domain accounts / Active Directory
```