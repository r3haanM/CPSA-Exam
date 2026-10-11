# VPN

IKEv1 has different ways of doing the initial setup. Two important ones are:

```ruby
IKEv1 (Internet Key Exchange) -> Before we send secret company traffic, let's first create a secure way for the two of us to talk

├── Main Mode
│
└── Aggressive Mode

One extra CREST point worth remembering:

UDP 500  = normal IKE/IPsec VPN negotiation

UDP 4500 = IPsec VPN when NAT traversal is being used

IPsec
UDP 500  → IKE negotiation
IP 50    → ESP
IP 51    → AH
```

Before two people exchange valuable packages, they first enter a private meeting room.

```objectivec
Internet
Client ---------------- Gateway
       Phase 1
Client =====secure===== Gateway
```

That secure relationship is called an:

**IKE SA**

```
IPsec

500 → IKE → SET UP

50  → ESP → ENCRYPT/PROTECT DATA

51  → AH  → AUTHENTICATE/INTEGRITY
```

SA = **Security Association**

Very important CREST term.

An SA basically describes:

```php
Who are we talking to? What encryption are we using? What integrity algorithm? 
What keys? How long are these keys valid?
```

---

> **“Four mandatory IKE Phase-1 attributes?”**
> 

immediately think:

✅ **Encryption + Hash + Authentication + DH Group**