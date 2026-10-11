# IP Address

When you see an IPv6 MCQ, first do this:

```nix
Does it have 8 groups?
        ↓
YES → probably valid

NO
 ↓
Does it contain :: ?
        ↓
YES → :: may fill missing zero groups
NO  → INVALID

2001:0db8:1428:57ab ❌
2001:db8::1428:57ab ✔️
2001:0db8:0000:0000:0000:0000:1428:57ab
2001:0db8:0:0::1428:57ab
2001:0db8:0:0:0:0:1428:57ab
```

Memorize this split:

```nix
IP OPTIONS
─────────────
Record Route   ← ⭐ remember this
Timestamp
Source Route

TCP OPTIONS
─────────────
MSS
Window Scale
SACK
TCP Timestamp
```