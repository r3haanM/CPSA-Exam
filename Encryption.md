# Encryption

### First classify what job it does

| Category | Easy meaning | Common examples |
| --- | --- | --- |
| **Symmetric encryption** | Same secret key encrypts and decrypts | AES, DES, 3DES, RC4, RC5, Blowfish |
| **Asymmetric encryption** | Public key + private key | RSA, ECC |
| **Hashing** | Creates a one-way fingerprint | MD5, SHA-1, SHA-256, SHA-512 |
| **Key agreement** | Helps two sides create a shared secret | Diffie-Hellman, ECDH |
| **Digital signature** | Proves who signed data + integrity | RSA signatures, DSA, ECDSA |

**Block cipher** = encrypts data in fixed-size chunks.

```nix
AES
↓
takes one fixed-size block
↓
encrypts it

Plaintext
    XOR
Keystream
    ↓
Ciphertext
```

CFB is **not the only mode that can make a block cipher operate in a stream-like way**.

Two others you should know are:

```nix
CFB → Cipher Feedback
OFB → Output Feedback
CTR → Counter mode
```

All can produce stream-like encryption behavior. But in **this MCQ**, only CFB is offered.

The especially useful distinction is:

```nix
ECB → independent blocks

CBC → chained blocks

CFB → ciphertext feedback → stream-like ✅

OFB → output feedback → stream-like

CTR → encrypt counters → keystream → stream-like
```

A useful bigger cheat sheet:

```nix
SYMMETRIC
AES
DES
3DES
RC4
RC5
Blowfish

ASYMMETRIC
RSA
DSA
ECC

KEY AGREEMENT
Diffie-Hellman
```

One important distinction: **Diffie-Hellman is not really an encryption cipher like RSA or AES**. Its main job is agreeing on shared key material, which is exactly what we discussed earlier.

```nix
AES      = symmetric, 128/192/256
DES      = symmetric, 56-bit
3DES     = symmetric, old
RC4      = stream cipher, old/weak
RSA      = asymmetric
DH       = key agreement, MITM without authentication
MD5      = 128-bit hash
SHA-1    = 160-bit hash
SHA-256  = 256-bit hash
ECB      = pattern leakage
CBC      = chained blocks
CFB      = stream-like feedback
CCMP     = AES-based Wi-Fi protection
```

### Symmetric cipher cheat sheet

| Algorithm | Type | Important CREST fact |
| --- | --- | --- |
| **DES** | Block cipher | **56-bit effective key**, obsolete |
| **3DES** | Block cipher | DES applied multiple times, obsolete |
| **AES** | Block cipher | **128 / 192 / 256-bit keys** |
| **RC4** | Stream cipher | Old, weak/obsolete; used by WEP/TKIP |
| **RC5** | Block cipher | Symmetric cipher |
| **Blowfish** | Block cipher | Symmetric, variable key length |

AES is a **block cipher**, but a mode determines how multiple blocks are processed.

| Mode | Think | Important point |
| --- | --- | --- |
| **ECB** | Each block alone | Weak; identical plaintext blocks produce identical ciphertext |
| **CBC** | Chain blocks | Uses previous ciphertext + IV |
| **CFB** | Cipher feedback | Makes block cipher act stream-like |
| **OFB** | Output feedback | Produces stream-like keystream |
| **CTR** | Counter | Encrypt counters to generate keystream |
| **GCM** | Modern authenticated mode | Encryption + integrity/authentication |