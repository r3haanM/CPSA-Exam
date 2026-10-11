# Compliance

### 🇬🇧 UK Govt

**Government Protective Marking Scheme** being tested in the question. The easiest way to read it is:

```nix
Lowest protection
      ↓
NPM (some protection needed)
PROTECT 
RESTRICTED (more sensitive)
CONFIDENTIAL
SECRET
TOP SECRET
      ↑
Highest protection
```

useful CREST-level summary is:

```
Computer Misuse Act 1990
        ↓
original UK computer-hacking law

Police and Justice Act 2006
        ↓
amended/strengthened the CMA
```

High level Summaries of ACTs covered in syllabus

| **Statute / Regulation** | **Core Focus** | **Key Sections / Articles** | **Maximum Penalties** | **Exam Trigger Scenario** |
| --- | --- | --- | --- | --- |
| **Computer Misuse Act 1990 (CMA)** | Unauthorized computer access, modification, and tool creation. | **Sec 1, 2, 3, 3ZA, 3A** | Up to **Life Imprisonment** (Sec 3ZA) | Scanning without authorization, running DoS payloads, exploiting unauthorized hosts. |
| **Police and Justice Act 2006** | Modernizing amendments to CMA 1990. | Amends **Sec 3**, adds **Sec 3A**, increases Sec 1 penalties. | Sec 1 raised to 2 yrs; Sec 3 to 10 yrs | "Which Act updated CMA to include Denial of Service and illegal tool making?" |
| **Serious Crime Act 2015** | Critical infrastructure and human safety protection. | Inserts **Sec 3ZA** into CMA 1990. | **Life Imprisonment** | Cyber-attack targeting hospital life-support, nuclear plants, or national power grids. |
| **Data Protection Act 1998 / 2018 & UK GDPR** | Lawful processing and storage of Personal Identifiable Information (PII). | **Art 5 (Principles)**, **Art 33 (Breach Reporting)**, **DPA 1998 (8 Principles)** | Greater of **£17.5M (€20M)** or **4% global turnover** | Dumping entire customer databases instead of minimal sanitized records; unencrypted evidence. |
| **Human Rights Act 1998** | Protection of fundamental human rights. | **Article 8** (Right to respect for private & family life, home, and correspondence). | Civil action / evidence admissibility challenge | Inadvertently opening or reading personal employee webmail on a target workstation. |
| **RIPA 2000 / IPA 2016** | Interception of telecommunications and packet data in transit. | **Lawful Business Practice Regulations (LBPR)** | Up to **2 years imprisonment** | Inline packet sniffing (promiscuous mode), VoIP tapping, or ARP spoofing without written consent. |
| **Copyright, Designs & Patents Act 1988 (CDPA)** | Intellectual property protection and software reverse engineering. | **Software Directive Exceptions** (Sec 50B/50BA) | Civil damages / criminal infringement | Decompiling commercial client binary files or copying proprietary code off-site without explicit permission. |
| **Communications Act 2003** | Misuse of public electronic communication networks. | **Section 127** (Improper / grossly offensive or menacing messages). | Up to **6 months imprisonment** / fine | Sending aggressive, threatening, or obscene spear-phishing pretexts to corporate staff. |
| **Sector Standard: PCI DSS 4.0** | Cardholder data environment (CDE) protection. | **Requirement 11.4** (Annual external/internal pentest & after significant changes). | Fines, loss of card processing capability | Frequency questions regarding e-commerce and card payment processing gateways. |

### Detailed Statutory Breakdown & Question Logic

#### 1. Computer Misuse Act 1990 (CMA)

- **Section 1: Unauthorised access to computer material**
    - *Actus Reus:* Causing a computer to perform any function (such as executing a command, probing a port, or logging in) to secure access to any program or data.
    - *Mens Rea:* Access is **unauthorized**, and the individual **knows** at the time that the access is unauthorized.
    - *Distinguishing Factor:* **No damage** is caused, and **no secondary crime** is intended.
    - *CREST Exam Traps:*
        - Scanning an IP address mistyped in the scoping contract is a Section 1 violation (lack of authorization).
        - There is **no "ethical hacking" or "public interest" defense** under English law for Section 1.
- **Section 2: Unauthorised access with intent to commit or facilitate commission of further offences**
    - *Actus Reus:* Committing an offense under Section 1.
    - *Mens Rea:* Carried out with the **specific intent** to commit or facilitate an arrestable offense carrying a sentence of 5+ years (e.g., Fraud Act 2006, Theft Act 1968, Blackmail).
    - *Distinguishing Factor:* The secondary offense does **not** need to be committed simultaneously; the intent alone triggers Section 2.
- **Section 3: Unauthorised acts with intent to impair, or with recklessness as to impairing, the operation of a computer**
    - *Actus Reus:* Doing an unauthorized act causing:
        1. Impairment of the operation of any computer.
        2. Prevention or hindering of access to any program or data.
        3. Impairment of the operation of any program or the reliability of any data.
    - *Mens Rea:* Either direct **intent** OR **recklessness**.
    - *Distinguishing Factor:* **Recklessness**. If a pentester runs an intrusive buffer overflow or unthrottled scanner without evaluating server stability and crashes production, they can be prosecuted under Section 3 due to reckless behavior.
- **Section 3ZA: Unauthorised acts causing, or creating significant risk of, serious damage**
    - *Added by:* Serious Crime Act 2015.
    - *Criteria for "Serious Damage":*
        - Damage to **human welfare** (loss of life, illness, or serious injury).
        - Damage to the **environment**.
        - Damage to the **economy of the United Kingdom**.
        - Damage to the **national security** of any country.
    - *Penalty:* Up to 14 years imprisonment, or **Life imprisonment** if human life is endangered or national security is compromised.
- **Section 3A: Making, supplying, or obtaining articles for use in computer misuse offences**
    - *Added by:* Police and Justice Act 2006.
    - *Subsections:*
        - *3A(1):* Making, adapting, supplying, or offering to supply an article **believing** it will be used in a CMA offense.
        - *3A(2):* Supplying or offering to supply an article **likely** to be used in an offense.
        - *3A(3):* Obtaining an article with a view to its being supplied or used to commit an offense.
    - *The Dual-Use Defense:* Security tools (Nmap, Metasploit, Wireshark, John the Ripper) have legitimate testing utilities. To breach Section 3A, the prosecution must prove **criminal intent or subjective belief** that the tool will be used maliciously.
- **Sections 4–9: Territorial Scope ("Significant Link")**
    - UK courts claim jurisdiction if **any single condition** is satisfied:
        1. The suspect was physically located in the UK at the time of the act.
        2. The targeted machine or data was located in the UK.
        3. Any intermediary server used to bounce, forward, or route the attack traffic was located in the UK.

#### 2. Police an Justice Act 2006 (Amending Statute)

Whenever an exam question tests what statute modified the original CMA 1990, remember these three changes:

- Substituted the word **"impairment"** for the older term *"modification"* in Section 3 to unequivocally include **Denial of Service (DoS)**.
- Inserted **Section 3A** into the CMA 1990 (criminalizing malicious tool creation/distribution).
- Increased Section 1 summary and indictment penalties (raising maximum prison term from 6 months to **2 years** on indictment).

#### 3. Data Protection Act (DPA 1998 / 2018 & UK GDPR)

Pentesters interact with customer databases containing Personally Identifiable Information (PII) during testing.

- **Core Principles (GDPR Article 5 / DPA 2018):**
    - **Data Minimisation:** You must only collect the minimum amount of personal data necessary to establish proof-of-concept. *Never download an entire database of 50,000 customers; extract 1–2 masked/redacted rows.*
    - **Integrity and Confidentiality (Security):** All evidence, screenshots, credential lists, and memory dumps must be stored in encrypted volumes (AES-256), transferred over secure encrypted channels, and securely destroyed following client sign-off.
    - **Storage Limitation:** Test data must not be retained longer than specified in the Statement of Work / retention schedule.
- **Role Definitions:**
    - **Data Controller:** The client organization (determines the purposes and means of processing personal data).
    - **Data Processor:** The security testing firm (processes personal data strictly on behalf of and according to written instructions from the controller).
- **Mandatory Breach Notification (Article 33):**
    - If personal data is compromised/exposed during or outside a test, notification to the **Information Commissioner's Office (ICO)** must occur within **72 hours** of becoming aware.

#### 4. Human Rights Act 1998 (HRA) — Article 8

- **Article 8 Protection:** *"Everyone has the right to respect for his private and family life, his home and his correspondence."*
- **The Pentesting Boundary:**
    - **Corporate vs. Private Material:** An employee's personal emails, private folders, personal photos, or personal browser history stored on or accessed via a corporate device fall under Article 8 protections.
    - **Action Protocol:** If a pentester gains access to a desktop or mail store and discovers personal communications, they **must not read or exfiltrate them**. The tester must pause, document the exposure path safely, and notify the primary client contact.

#### 5. RIPA 2000 & Investigatory Powers Act 2016 (IPA)

- **Core Offense:** Unlawful intentional interception of communications in the course of transmission (wired, wireless, packet-switched).
- **When Pentesters Run Afoul:**
    - Setting up inline network taps or ARP cache poisoning to sniff cleartext passwords without explicit scope authorization.
    - Capturing VoIP telephone traffic or intercepting external employee telecommunications.
- **Lawful Business Practice Regulations (LBPR) 2000:**
    - Permits businesses to monitor and record communications on their own networks without caller/employee consent for specific authorized purposes (e.g., verifying regulatory compliance, checking system security, preventing crime). Pentesters rely on the client's authority under LBPR in the written contract.

#### 6. Sector-Specific Regulations & Standards

- **PCI DSS 4.0 (Payment Card Industry):**
    - **Requirement 11.4:** Penetration testing must be performed at least **once every 12 months** and after any **significant application or infrastructure change**.
    - **CDE Scope Reduction Testing:** Pentesters must conduct specific network segmentation tests to verify that the Cardholder Data Environment (CDE) is strictly isolated from non-sensitive corporate networks.
- **NIS Regulations 2018:**
    - Regulates Operators of Essential Services (energy, water, health, transport) and Relevant Digital Service Providers (search engines, cloud infrastructure).
    - Requires incident notifications to competent authorities within **72 hours**.

### Rapid Decision Flowchart: How to Answer Any Legal MCQ

```
[Is the target system or data accessed/modified without permission?]
       │
       ├─► YES ──► Was there ANY physical damage, crash, or service degradation?
       │            │
       │            ├─► YES ──► Did it endanger critical infrastructure or human life?
       │            │            │
       │            │            ├─► YES ──► Computer Misuse Act 1990 — Section 3ZA (Life Max)
       │            │            └─► NO  ──► Computer Misuse Act 1990 — Section 3 (Impairment / Recklessness)
       │            │
       │            └─► NO  ──► Was there intent to commit fraud, theft, or serious crime?
       │                         │
       │                         ├─► YES ──► Computer Misuse Act 1990 — Section 2
       │                         └─► NO  ──► Computer Misuse Act 1990 — Section 1 (Unauthorized Access)
       │
       └─► NO (Target is In-Scope with signed contract)
            │
            ├─► Did the tester distribute or build dedicated exploit/malware code for malicious use?
            │   └──► Computer Misuse Act 1990 — Section 3A (Articles for Misuse)
            │
            ├─► Did the tester read personal emails or private files of an employee?
            │   └──► Human Rights Act 1998 — Article 8 (Right to Privacy / Correspondence)
            │
            ├─► Did the tester dump excessive, unmasked customer PII or store it unencrypted?
            │   └──► Data Protection Act 2018 / UK GDPR (Breach of Data Minimisation / Security)
            │
            ├─► Did the tester sniff network traffic / ARP poison without explicit client consent?
            │   └──► Regulation of Investigatory Powers Act 2000 / IPA 2016 (Interception)
            │
            └─► What amended Section 3 to include DoS and created Section 3A?
                └──► Police and Justice Act 2006
```