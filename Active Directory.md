# Active Directory

- **Schema Master**
    
    Controls changes to the Active Directory **schema**. Schema simply means the blueprint describing what types of objects and attributes AD can contain.
    
    ```
    "What fields can a User object have?"
    "What properties can a Computer object have?"
    ```
    
- **Domain Naming Master**
Controls adding/removing **domains** in the forest.
- **RID Master**
    
    **RID = Relative Identifier.** Helps Domain Controllers generate unique SIDs for users/groups/computers. Think:
    
    ```
    New user
       ↓
    needs unique SID
       ↓
    RID Master helps supply unique RID ranges
    ```
    
- **PDC Emulator**
    
    **PDC = Primary Domain Controller.** This role is extremely important. It handles things such as:
    
    password-change priority, time synchronization responsibilities, and compatibility with older Windows domain behavior.
    
- **Infrastructure Master**
Helps keep references to objects in other domains up to date.

The five roles are divided like this:

```
FOREST-WIDE
───────────
Schema Master
Domain Naming Master

DOMAIN-WIDE
───────────
RID Master
PDC Emulator
Infrastructure Master
```

Meaning:

> One Schema Master and one Domain Naming Master per **forest**.
> 

But:

> RID Master, PDC Emulator, and Infrastructure Master exist once per **domain**.
> 

So if a forest has 3 domains:

```
Forest
├── Domain A
├── Domain B
└── Domain C
```

you have:

```
2 forest-wide FSMO roles total

+

3 domain-wide roles × 3 domains
= 9

Total FSMO role instances = 11
```