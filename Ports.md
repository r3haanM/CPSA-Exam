# Ports

**UDP 137 = NetBIOS Name Service**

*Memorize these:*

```nix
UDP 137
NetBIOS Name Service
"Who is COMPUTER-X?"

UDP 138
NetBIOS Datagram Service
Send connectionless NetBIOS messages

TCP 139
NetBIOS Session Service
Create an actual connection/session
```

And one extra Windows networking port:

```nix
TCP 445
SMB directly over TCP/IP
```

*This little group is **very useful for CREST**:*

```nix
123 UDP = NTP
137 UDP = NAME
138 UDP = DATAGRAM
177 UDP = XDMCP (X Display Manager Control Protocol)
139 TCP = SESSION
445 TCP = SMB
79  TCP = FINGER
37 TCP/UDP = TIME
13 TCP/UDP = daytime
1433 = MSSQL
2433 = hidden MSSQL
1434 = SQL discovery
3306 = MySQL
1521 = Oracle
```

For CREST, this grouping is useful:

```nix
TCP 512 → rexec
TCP 513 → rlogin
UDP 513 -> rwho
TCP 514 → rsh
```

**"Port 47" vs "Protocol 47”**

```nix
TCP port 80
TCP port 443
TCP port 1723
UDP port 53

IP Protocol 6  → TCP
IP Protocol 17 → UDP
IP Protocol 47 → GRE
IP Protocol 50 → ESP
IP Protocol 51 → AH
IP Protocol 89 → OSPF
IP Protocol 94 → IPIP (IP-within-IP Encapsulation Protocol) 
```

Ref: https://www.iana.org/assignments/protocol-numbers 

Memorize:

> **Below 1024 = privileged ports**
> 

or more exactly:

> **0–1023**
> 

### SNMP - UDP `161`

The important pattern is:

```
1.3.6.1.4.1.<vendor>
```

`1.3.6.1.4.1` = **private enterprise/vendor area** of the SNMP tree. The **next number identifies the vendor**. IBM's documentation describes exactly this structure.

A useful common-vendor cheat sheet is:

| Vendor | Enterprise OID | Number to recognize |
| --- | --- | --- |
| **IBM** | `1.3.6.1.4.1.2` | **2** |
| **Cisco** | `1.3.6.1.4.1.9` | **9** |
| **HP / HPE** | `1.3.6.1.4.1.11` | **11** |
| **Microsoft** | `1.3.6.1.4.1.311` | **311** |
| **Dell** | `1.3.6.1.4.1.674` | **674** |
| **Huawei** | `1.3.6.1.4.1.2011` | **2011** |
| **Check Point** | `1.3.6.1.4.1.2620` | **2620** |
| **Juniper** | `1.3.6.1.4.1.2636` | **2636** |
| **F5** | `1.3.6.1.4.1.3375` | **3375** |
| **Schneider Electric** | `1.3.6.1.4.1.3883` | **3883** |
| **Fortinet** | `1.3.6.1.4.1.12356` | **12356** |
| **Palo Alto Networks** | `1.3.6.1.4.1.25461` | **25461** |
| **Arista** | `1.3.6.1.4.1.30065` | **30065** |

**Ques: What would an SNMP request to set OID 1.3.6.1.4.1.9.2.1.55.10.0.0.1  to "file" on a Cisco router using a community string with read/write access do?**

> *“You have write access to SNMP on a Cisco router. You tell its `writeNet` function to send its configuration to TFTP server 10.0.0.1 and name the file `file`. What happens?”*
> 

Now the answer becomes obvious:

```
Cisco Router
     |
     | configuration
     |
     v
TFTP Server
10.0.0.1

saved as:
file
```

✅ **The router uploads/sends its configuration to the TFTP server at `10.0.0.1` as a file named `file`.**