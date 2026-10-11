# Command

`finger user@hostname` ⇒ 

(Hey `hostname`, give me finger information about `user` )

The `/etc/passwd` file contains user-account information.

```nix
bob:x:1001:100:Bob Smith:/home/bob:/bin/sh

Empty GECOS:

bob:x:1001:100::/home/bob:/bin/sh
                  ↑
               nothing
bob
 ↓
username

x
 ↓
password placeholder

1001
 ↓
UID (user ID)

100
 ↓
GID (group ID)

Bob Smith
 ↓
**GECOS field**

/home/bob
 ↓
home directory

/bin/sh
 ↓
login shell
```

## **Packages Listing (Different Linux Distros)**

Solaris
→ `pkginfo`

Debian / Ubuntu
→ `dpkg` 

Red Hat-family
→ `rpm`

Traditional BSD
→ `pkg_info`

use this short CREST cheat sheet:

- **`cmsd, kcms_server, sadmind, snmpXdmid`** → **Solaris management/RPC services** ✅
- **`telnet, ssh, ftp, http`** → normal remote access / file / web protocols ❌
- **`rexec, rlogin, rsh, rsync`** → old Unix remote-access/file tools, not all RPC ❌
- **`ntp, nfs, netbios, nntp`** → mixed protocols; only NFS is strongly RPC-associated ❌
- **`rusersd, rstatd, sprayd, rexec`** → first 3 are RPC-style services, but plain `rexec` is the trap ❌

Best memory trick:

> **Solaris weird daemon names = think RPC.**
> 
> 
> `sadmind`, `cmsd`, `rstatd`, `rusersd`, `sprayd` are strong RPC clues.
> 

And remember this trap:

```
rexec     ≠ RPC service
rpc.rexd  = RPC service
```

So if the question says **“all ONC/RPC services”**, one non-RPC item makes the whole option wrong.

**`pkginfo` = installed software packages**

**`showrev` = system revision / installed patch information**

practical grouping:

```nix
RPM FAMILY
RHEL / Fedora / CentOS / Rocky / Alma / SUSE
→ rpm -qa

DEB FAMILY
Debian / Ubuntu / Kali / Mint
→ dpkg -l

ARCH FAMILY
Arch / Manjaro
→ pacman -Q

ALPINE
→ apk info

SOLARIS
→ pkginfo

FREEBSD
→ pkg info
```

**NBT = NetBIOS over TCP/IP.**

```nix
nbtstat or better nbtscan -> obtain a list of systems from a master browser
, together with details about the version and available services
```

---

| Choice | Representative capture line | Instant clue |
| --- | --- | --- |
| **STP** | `17:57:57.850175 802.1d config 8064.00:0c:85:f1:3f:80 root 8064.00:0b:46:48:29:80 pathcost 19 age 1 max 20 hello 2 fdelay 15` | `802.1d`, `root`, `pathcost`, `hello`, `fdelay` |
| **CDP** | `17:58:01.396446 CDPv2, ttl: 180s, Device-ID 'switch01.company.com', length 404` | `CDP/CDPv2`, `Device-ID`, `ttl` |
| **ARP** | `17:58:05.123456 ARP, Request who-has 192.168.1.1 tell 192.168.1.20, length 28` | `ARP`, `who-has`, `tell` |
| **NetBIOS Name Service** | `16:23:57.094021 IP 192.168.124.204.137 > 192.168.124.255.137: NBT UDP PACKET(137): QUERY; REQUEST; BROADCAST` | `UDP 137`, `NBT`, `QUERY`, `BROADCAST` |
| **NetBIOS Session Service** | `17:58:20.123456 IP 192.168.1.20.49152 > 192.168.1.30.139: Flags [S], seq ..., win ..., length 0` | `TCP 139`, TCP flags/session |