# Linux

### FTP

Memorize:

> **`ftpusers` sounds like “FTP users,” but it actually means users forbidden from FTP.**
> 

Even better:

```
/etc/ftpusers
= FTP DENY list
```

And if you see `root` inside it:

```
root cannot log in via FTP
```

```
SSH v1
→ old / insecure

SSH v2
→ confidentiality + integrity + authentication ✅

SSH v3
→ distractor

Telnet
→ plaintext

IPsec AH
→ integrity/authentication only
→ no encryption

IPsec ESP
→ encryption + integrity/authentication
```

### File System

For a `normal file`:

```
r = read the file
w = modify the file
x = execute/run the file
```

But for a **`directory`**:

```
r = list the names inside it
w = create/delete/rename entries inside it
x = traverse/access through it
```