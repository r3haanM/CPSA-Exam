# Networking

### Master Protocol, Header & Wireless Reference Table

| **Protocol / Standard** | **Port / Layer / Mode** | **Key Header Fields / Specs** | **Security Vulnerability / Mechanism** | **Exam Scenario / CREST Trigger** |
| --- | --- | --- | --- | --- |
| **IPv4** | Layer 3 (Network) | Min 20B (up to 60B); Identification, Flags, Fragment Offset, TTL, Protocol. | Fragment reassembly abuse (Teardrop, tiny fragments bypassing stateless ACLs). | "What happens when the DF flag is set and packet size exceeds MTU?" → Dropped, returns ICMP Type 3 Code 4. |
| **IPv6** | Layer 3 (Network) | Fixed 40B; Next Header (replaces Protocol), Hop Limit (replaces TTL), Flow Label. | Extension Header manipulation, SLAAC spoofing, Rogue RA (Router Advertisements). | Header comparison questions: no checksum, fixed length, fragmentation handled strictly by source (not routers). |
| **TCP** | Layer 4 (Transport) | Min 20B (up to 60B); Flags (URG, ACK, PSH, RST, SYN, FIN), Window Size, Seq/Ack Nos. | SYN Flood (resource exhaustion), TCP reset attacks, predictable initial sequence numbers (ISN). | 3-way handshake analysis (`SYN` → `SYN-ACK` → `ACK`), FIN wait states, MSS negotiations. |
| **UDP** | Layer 4 (Transport) | Fixed 8B; Source Port, Destination Port, Length, Checksum. | Connectionless; susceptible to IP spoofing, reflection/amplification (NTP, DNS, SNMP). | Port scanning: open/filtered returns no response; closed returns ICMP Type 3 Code 3 (Port Unreachable). |
| **ICMP** | Layer 3 (Control) | Type (8-bit), Code (8-bit), Checksum, Header + 8 bytes of original datagram. | Covert channels, ICMP redirect spoofing, Ping of Death, network mapping (`traceroute`). | Match exact Type/Code pairs to firewall filter states and network diagnostic responses. |
| **SNMP (v1/v2c vs v3)** | UDP 161 (Poll) / UDP 162 (Trap) | MIB hierarchy, OIDs (`1.3.6.1...`), Community Strings (v1/v2c), USM (v3). | v1/v2c sends `public`/`private` community strings in cleartext; v3 provides auth (SHA/MD5) and privacy (AES/DES). | Terminal output showing SNMP enumeration of network interfaces, routing tables, and installed software. |
| **Cisco CDP / Layer 2** | Layer 2 (Data Link) | Broadcasts every 60s; reveals device model, OS version, native VLAN, IP address. | Cleartext multicast broadcasts (no auth); enables rapid targeted recon and VLAN discovery. | Passive sniffing reveals router/switch OS revisions without sending a single active packet. |
| **VLANs (802.1Q)** | Layer 2 | 4-byte VLAN tag (TPID `0x8100`, TCI: Priority, DEI, 12-bit VLAN ID). | Switch Spoofing (exploiting DTP auto/desirable) and Double Tagging (stripping outer native VLAN tag). | "How does Double Tagging work?" $\rightarrow$ Packet contains two tags; switch strips the native VLAN tag; second switch routes inner tag. Unidirectional only. |
| **ARP / DHCP** | Layer 2 / Layer 7 (UDP 67/68) | Hardware type, Protocol type, Opcode (ARP Request=1, Reply=2). | ARP cache poisoning (MITM), DHCP Starvation (MAC spoofing), Rogue DHCP Rogue Gateway. | Sniffing credentials via ARP poisoning; mitm6/rogue DHCP assigning attacker-controlled DNS servers. |
| **802.11 WEP** | Layer 2 Wireless | 64-bit or 128-bit key; RC4 stream cipher; 24-bit cleartext Initialization Vector (IV). | 24-bit IV collision weakness; weak key scheduling in RC4; crackable offline within minutes (FMS / KoreK). | Legacy wireless scenario: rapid mathematical key recovery due to small IV size and IV reuse. |
| **802.11 WPA (WPA-TKIP)** | Layer 2 Wireless | RC4 cipher; Temporal Key Integrity Protocol (TKIP); 48-bit IV; Michael 64-bit MIC. | Beck-Tews / Ohigashi-Kawano attacks against Michael MIC; replaced by WPA2. | Transitional wireless standard using dynamic per-packet keys and Michael MIC over legacy WEP hardware. |
| **802.11 WPA2 (AES-CCMP)** | Layer 2 Wireless | AES cipher; CCMP (Counter Mode CBC-MAC Protocol); 4-way EAPOL handshake. | Pre-shared Key (PSK) vulnerable to offline dictionary attack if 4-way handshake (EAPOL frames) is captured. | Extracting `ANonce`, `SNonce`, `MIC` from packet capture to crack PSK offline using a wordlist. |
| **802.1X / Enterprise** | Layer 2 / Port Auth | Supplicant, Authenticator (AP), Authentication Server (RADIUS). EAP methods. | Rogue AP / Evil Twin harvests MSCHAPv2 hashes if EAP-PEAP client validation is misconfigured. | Distinguishing between EAP-TLS (mutual certs, most secure) vs. PEAP/EAP-TTLS (server cert only, tunnelled inner auth). |
| **IPsec (AH vs. ESP)** | Network Layer VPN | Protocol 51 (AH), Protocol 50 (ESP), UDP 500 (IKE), UDP 4500 (NAT-T). | AH provides authentication/integrity but **no encryption**; ESP provides encryption + authentication. | "Which IPsec header must be selected to guarantee confidentiality?" $\rightarrow$ ESP (Protocol 50). |

### Detailed Technical Breakdown & Exam Scenarios

#### 1. IP Protocols & Header Mechanics

- **IPv4 Header Fields Tested:**
    - **IHL (Internet Header Length):** 4-bit field measuring header length in 32-bit (4-byte) words. Minimum is 5 (5 $\times$ 4 = 20 bytes); maximum is 15 (60 bytes).
    - **Fragmentation Fields:**
        - *Identification:* 16-bit value grouping fragments of the same original datagram.
        - *Flags:* 3 bits (Bit 0: Reserved; Bit 1: **DF** = Don't Fragment; Bit 2: **MF** = More Fragments).
        - *Fragment Offset:* 13-bit value measuring fragment placement in 8-byte blocks.
    - **TTL (Time to Live):** Decremented by 1 at every router hop. When TTL reaches 0, the router drops the packet and responds with an **ICMP Type 11 Code 0 (Time-to-live exceeded in transit)**. This mechanic powers `traceroute`.
- **IPv6 vs. IPv4 Differences:**
    - IPv6 header is a **fixed 40 bytes** (simplifies router hardware processing).
    - IPv4 **TTL** is renamed to **Hop Limit**.
    - IPv4 **Protocol** field is replaced by **Next Header** (which points to TCP, UDP, or extension headers like routing, hop-by-hop, or fragmentation).
    - IPv6 has **no header checksum** (relies on Layer 2 and Layer 4 checksums).
    - In IPv6, intermediate routers **never fragment packets**; fragmentation is handled exclusively by the sending host using Path MTU Discovery.

#### 2. TCP Handshakes, Flags & Port Scanning Responses

- **TCP 3-Way Handshake & Teardown:**
    - *Establishment:* `SYN` (Seq=x) $\rightarrow$ `SYN-ACK` (Seq=y, Ack=x+1) $\rightarrow$ `ACK` (Seq=x+1, Ack=y+1).
    - *Graceful Teardown:* `FIN` $\rightarrow$ `ACK` $\rightarrow$ `FIN` $\rightarrow$ `ACK`.
    - *Abrupt Reset:* A single packet with the `RST` flag immediately terminates the connection without waiting for outstanding data.
- **Port Scanning Packet Response Matrix (Crucial for Output Interpretation):**

| **Scan Type** | **Nmap Flag** | **Sent Packet Flags** | **Target Open Port Response** | **Target Closed Port Response** | **Target Filtered (Firewall Drop)** |
| --- | --- | --- | --- | --- | --- |
| **TCP Connect** | `-sT` | Full Handshake (`SYN`) | Completes handshake (`SYN-ACK` $\rightarrow$ `ACK` $\rightarrow$ `RST`) | `RST-ACK` | No response or ICMP Type 3 |
| **SYN (Half-Open)** | `-sS` | `SYN` | `SYN-ACK` (Scanner replies with `RST`) | `RST` | No response or ICMP Type 3 |
| **UDP Scan** | `-sU` | Empty UDP packet | No response (Open/Filtered) OR UDP application payload response | **ICMP Type 3 Code 3** (Port Unreachable) | No response (or ICMP Type 3 Code 1, 2, 9, 10, 13) |
| **FIN Scan** | `-sF` | `FIN` | No response (RFC 793 standard) | `RST-ACK` | No response |
| **Null Scan** | `-sN` | None (all flags 0) | No response (RFC 793 standard) | `RST-ACK` | No response |
| **Xmas Scan** | `-sX` | `FIN, PSH, URG` | No response (RFC 793 standard) | `RST-ACK` | No response |
- **Exam Trap (Windows vs. RFC 793):**
    - Systems complying with RFC 793 (Linux, Unix, BSD) drop inverted scans (`FIN`, `Null`, `Xmas`) on **open** ports and return `RST` on **closed** ports.
    - **Microsoft Windows, Cisco IOS, and legacy BSD implementations violate RFC 793**; they return a `RST` to `FIN`/`Null`/`Xmas` probes regardless of whether the port is open or closed, making these scans ineffective against Windows hosts.

#### 3. ICMP Types and Codes Reference

CREST tests specific numeric ICMP Type and Code combinations:

| **Type** | **Code** | **Description** | **Technical Cause / Meaning** |
| --- | --- | --- | --- |
| **0** | 0 | **Echo Reply** | Response to a ping request (Type 8). |
| **3** | 0 | **Net Unreachable** | Routing failure; gateway cannot locate destination network. |
| **3** | 1 | **Host Unreachable** | Final router cannot locate the destination IP (ARP failed). |
| **3** | 2 | **Protocol Unreachable** | Transport protocol (e.g., GRE, IGMP) not supported on target. |
| **3** | 3 | **Port Unreachable** | Target host closed port response to a **UDP datagram**. |
| **3** | 4 | **Fragmentation Needed (DF Set)** | Packet exceeds path MTU and Don't Fragment bit is enabled. |
| **3** | 13 | **Communication Administratively Prohibited** | Packet explicitly dropped by a **firewall / ACL rule**. |
| **8** | 0 | **Echo Request** | Standard ping request packet. |
| **11** | 0 | **Time to Live Exceeded in Transit** | TTL decremented to 0 by an intermediate hop (`traceroute`). |
| **11** | 1 | **Fragment Reassembly Time Exceeded** | Fragmented datagram reassembly timed out on destination host. |
| 10  | 0 | Router Solicitation |  |

#### 4. Subnetting & CIDR Rapid Calculation

You will encounter quick network size and boundary questions:

$$\text{Total Addresses} = 2^{(32 - \text{CIDR})}, \quad \text{Usable Hosts} = 2^{(32 - \text{CIDR})} - 2$$

*(Subtract 2 for Network ID [all host bits 0] and Broadcast Address [all host bits 1]).*

| **CIDR Prefix** | **Subnet Mask** | **Total IPs** | **Usable Hosts** | **Block Size (Jump)** |
| --- | --- | --- | --- | --- |
| **/24** | 255.255.255.0 | 256 | 254 | 256 |
| **/25** | 255.255.255.128 | 128 | 126 | 128 |
| **/26** | 255.255.255.192 | 64 | 62 | 64 |
| **/27** | 255.255.255.224 | 32 | 30 | 32 |
| **/28** | 255.255.255.240 | 16 | 14 | 16 |
| **/29** | 255.255.255.248 | 8 | 6 | 8 |
| **/30** | 255.255.255.252 | 4 | **2** (Point-to-Point WAN) | 4 |
| **/31** | 255.255.255.254 | 2 | 2 (RFC 3021 Point-to-Point) | 2 |
| **/32** | 255.255.255.255 | 1 | Single Host Route | 1 |
- **Exam Shortcut:** If asked for the broadcast address of `192.168.10.45/27`:
    - Block size for `/27` is $256 - 224 = 32$.
    - Subnets: `.0`, `.32`, `.64`...
    - Target IP `.45` falls in the `.32` block.
    - Network Address = `192.168.10.32`.
    - Broadcast Address = `192.168.10.63` (one below the next subnet `.64`).

#### 5. Infrastructure Protocols & Layer 2 Exploitation

- **SNMP (Simple Network Management Protocol):**
    - **Ports:** UDP 161 (Manager-to-Agent requests), UDP 162 (Agent-to-Manager Traps).
    - **v1 & v2c:** Completely unencrypted; uses plaintext **Community Strings** (`public` for read-only, `private` for read-write).
    - **v3 Security Models (USM):**
        - *noAuthNoPriv:* No authentication, no encryption (community string style).
        - *authNoPriv:* Authentication via MD5 or SHA; transmission unencrypted.
        - *authPriv:* Authentication (MD5/SHA) **and** Privacy/Encryption (DES or AES).
    - **Dangerous OIDs:** MIB-II `1.3.6.1.2.1` exposes full system description, routing tables (`ipRouteTable`), network interfaces, and running software.
- **Layer 2 Attacks & Countermeasures:**
    - **CAM Table Overflow / MAC Flooding:** Flooding switch with bogus MAC addresses fills the Content Addressable Memory table. Once full, the switch defaults to failing open as a **hub**, broadcasting all frames to every port. *Mitigation: Port Security (limiting MACs per port).*
    - **ARP Spoofing / Poisoning:** Sending gratuitous ARP replies mapping the target IP (default gateway) to the attacker's MAC address. *Mitigation: Dynamic ARP Inspection (DAI) combined with DHCP Snooping.*
    - **DHCP Starvation:** Exhausting the DHCP server scope by rapidly requesting leases with spoofed MAC addresses, followed by deploying a Rogue DHCP server. *Mitigation: DHCP Snooping.*
    - **VLAN Hopping:**
        - *Switch Spoofing:* Connecting to an access port configured with Dynamic Trunking Protocol (DTP) set to `auto` or `desirable`, sending DTP packets to turn the port into a trunk, and accessing all VLANs. *Mitigation: Disable DTP (`switchport nonegotiate`).*
        - *Double Tagging:* Attacker on native VLAN sends frame with two 802.1Q tags. The first switch strips the outer native VLAN tag and forwards the inner tag to the second switch, routing it into the target VLAN. *Unidirectional only.* *Mitigation: Change the native VLAN on all trunks to an unused VLAN.*

#### 6. Wireless Security & VPN Architectures

- **Wireless Standards Progression:**
    - **WEP:** 64-bit (40-bit key + 24-bit IV) or 128-bit (104-bit key + 24-bit IV). RC4 stream cipher. Vulnerable because a 24-bit IV allows IV reuse after $\approx 16.7$ million packets, exposing the key to FMS/KoreK algebraic recovery.
    - **WPA:** Replaced WEP's static key scheduling with **TKIP** (Temporal Key Integrity Protocol), mixing keys per packet, and added the **Michael** Message Integrity Code (MIC).
    - **WPA2:** Mandated **AES-CCMP** (Counter Mode CBC-MAC Protocol), providing strong authenticated encryption.
    - **WPA/WPA2 4-Way Handshake:**
        1. Authenticator (AP) $\rightarrow$ Supplicant (Client): `ANonce`.
        2. Supplicant $\rightarrow$ Authenticator: `SNonce` + `MIC`.
        3. Authenticator $\rightarrow$ Supplicant: GTK (Group Temporal Key) + `MIC`.
        4. Supplicant $\rightarrow$ Authenticator: `ACK`.
        - *The Attack:* Capturing messages 1 and 2 yields both nonces (`ANonce`, `SNonce`) and the `MIC`. This allows attackers to run an offline dictionary attack to compute the Pairwise Master Key (PMK) and match the MIC without sending further packets.
- **802.1X Enterprise EAP Methods:**
    - **EAP-TLS:** Mutual authentication using digital certificates on both the client (supplicant) and server (RADIUS). Highly secure; resistant to rogue AP attacks.
    - **PEAP (Protected EAP):** Server authenticates via certificate, establishing a TLS tunnel. The client authenticates inside the tunnel via MSCHAPv2. Vulnerable to **Evil Twin / Rogue AP** attacks if client endpoints do not enforce strict root CA certificate validation.
- **IPsec Architecture (Layer 3 VPN):**
    - **AH (Authentication Header - Protocol 51):** Provides authentication, integrity, and replay protection. **Provides ZERO encryption (confidentiality).** Incompatible with NAT because AH hashes the IP header (which NAT modifies).
    - **ESP (Encapsulating Security Payload - Protocol 50):** Provides **encryption (confidentiality)**, integrity, authentication, and anti-replay protection.
    - **IKE (Internet Key Exchange - UDP Port 500):**
        - *Phase 1:* Establishes the secure channel (ISAKMP SA). Operates in **Main Mode** (6 messages, protects identity) or **Aggressive Mode** (3 messages; sends user/hash credentials in the clear, vulnerable to offline cracking).
        - *Phase 2:* Establishes IPsec SAs for bulk data transfer (Quick Mode).
        - *NAT-Traversal (NAT-T):* Encapsulates ESP inside **UDP Port 4500** to pass through stateful NAT firewalls.

### Rapid Troubleshooting & Identification Chart

```
[Is the scan or network probe behaving unexpectedly?]
       │
       ├─► Probe: Empty packet to UDP port ──► Returns: ICMP Type 3 Code 3?
       │   └──► Answer: Port is CLOSED. (If NO response returned, port is OPEN/FILTERED).
       │
       ├─► Probe: FIN or NULL scan ──► Target returns RST on ALL ports?
       │   └──► Answer: Target is a WINDOWS OS or stateful firewall (violates RFC 793).
       │
       ├─► Probe: Ping packet ──► Returns: ICMP Type 3 Code 13?
       │   └──► Answer: Blocked by an intermediate FIREWALL or ACCESS CONTROL LIST.
       │
       ├─► Probe: traceroute hop ──► Returns: ICMP Type 11 Code 0?
       │   └──► Answer: Hop router dropped packet because TTL expired (TTL = 0).
       │
       ├─► Wireless: Captured EAPOL frames containing ANonce, SNonce, and MIC?
       │   └──► Answer: WPA/WPA2 4-Way Handshake captured; crackable OFFLINE via dictionary attack.
       │
       └─► VPN: IPsec traffic passing through NAT gateway without breaking?
           └──► Answer: NAT-Traversal (NAT-T
           
           
```

| **Prefix / Block** | **Subnet Mask** | **Start IP** | **End IP** | **Total Addresses** |
| --- | --- | --- | --- | --- |
| **`10.0.0.0/8`** | `255.0.0.0` | `10.0.0.0` | `10.255.255.255` | 16,777,216 |
| **`172.16.0.0/12`** | `255.240.0.0` | `172.16.0.0` | `172.31.255.255` | 1,048,576 |
| **`192.168.0.0/16`** | `255.255.0.0` | `192.168.0.0` | `192.255.255.255` | 65,536 |

### The 3 Private IP Rules

On the exam, a "private IP" (RFC 1918) only ever starts with one of these three patterns:

1. Starts with **`10.`** (anything starting with 10, like `10.0.0.1` or `10.200.5.1`).
2. Starts with **`192.168.`** (like your home Wi-Fi router).
3. Starts with **`172.`** — but **only** if the second number is between **16 and 31** (like `172.16.x.x`, `172.20.x.x`, or `172.31.x.x`).

That is the entire rule.

Look at the **second character** of the MAC address:
• If it is an **Even** number/letter (0, 2, 4, 6, 8, A, C, E) $\rightarrow$ It is **Unicast** (sent to one device).
• If it is an **Odd** number/letter (1, 3, 5, 7, 9, B, D, F) $\rightarrow$ It is **Multicast/Broadcast** (sent to a group).