# Services

There are two files you should associate with **Berkeley R-services**:

```nix
/etc/hosts.equiv
        ↓
SYSTEM-WIDE trust = "What machines/users does this SYSTEM trust?"

~/.rhosts
        ↓
ONE USER'S trust = "What machines/users does THIS USER trust?"

❌ hosts.allow
TCP Wrappers historically controlled which hosts could access certain network services.
/etc/hosts.allow
/etc/hosts.deny

For CREST, this grouping is useful:

TCP 512 → rexec
TCP 513 → rlogin
TCP 514 → rsh

.rhosts       = ONE USER trusts remote hosts/users
hosts.equiv   = WHOLE SYSTEM trusts remote hosts/users
hosts.allow   = network service access control
```