# Wifi

### WEP

WEP uses **RC4** encryption. It combines:

```perl
24-bit IV
+
secret WEP key
```

For classic 64-bit WEP:

```pascal
24-bit IV + 40-bit secret key = 64 bits total
```

For 128-bit WEP:

```
24-bit IV + 104-bit secret key = 128 bits total
```

```
WEP
→ RC4
→ IV = 24 bits ⭐

64-bit WEP
→ 40-bit key + 24-bit IV

128-bit WEP
→ 104-bit key + 24-bit IV

Small/reused IVs
→ major WEP weakness
```

```
WEP
 ↓
24-bit IV
 ↓
too few IV values
 ↓
IVs repeat
 ↓
weaknesses in RC4/WEP key generation become exploitable
 ↓
WEP key can be recovered
```