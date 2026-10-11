# Permission

`umask` removes permissions from newly created files/directories. It does not add permissions
Unix permissions use three main groups:

```nix
Owner | Group | Others

4 = Read
2 = Write
1 = Execute

0 0 2 7
  │ │ │
  │ │ └── Others
  │ └──── Group
  └────── Owner
  
umask 027

Owner  → remove nothing
Group  → remove write
Others → remove everything
```