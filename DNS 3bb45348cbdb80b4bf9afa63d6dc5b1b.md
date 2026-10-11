# DNS

**BIND = Berkeley Internet Name Domain.** It is very common **DNS server software**.

`dig @nameserver version.bind txt chaos` 

```nix
dig
↓
ask this DNS server

version.bind
↓
"What BIND version are you?"

TXT
↓
return text

CHAOS
↓
special DNS class
```