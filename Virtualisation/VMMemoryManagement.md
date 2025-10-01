# Memory Management in Virtualisation

## In normal OS
 - Each process has its own **Virtual Memory (VA)** space.
 - The OS maintains a page table for each process, mapping **VA** -> **PA**
 - CPU MMU uses that PT to translate from VA to PA.
 - CPU uses **CR3 register** to point to the current process’s page table **root**.

## Problem In virtualisation:
 - A **Guest OS** thinks it has full control over physical memory (illusion)
 - The Guest OS creates its own page tables:
    - Mapping **GVA** -> **GPA**
 - Hoever GPA is fake and do not correspond to  correct physical memory addresses
 - The CPU MMU cannot use the them directly

 ## (Old) Software solution - Shadow Page Tables (SPT)
 - Guest OS creates page tables as normal (non-privileged action)
 - As soon as Guest OS tries to activate PT by loading CR3, or edit PTEs (privileged actions) --> CPU `trap`
 - VMM takes over control, reads the Guest’s PT entry (e.g., "`GVA 0x1000` → `GPA 0x2000`"), looks up where `GPA 0x2000` actually resides in HPA
 - VMM writes a new entry in a shadow PT: \
  `GVA 0x1000` → `HPA 0xA0000`
 - The real CPU never consults the guest PT directly. It only consults the shadow PT.
 So when a guest process later accesses a GVA, the CPU walks the shadow PT and lands directly at HPA

**In SPT: Guest PT is just there to provide Guest OS with illusion of hardware — only the shadow PT is actually used by the CPU.**

 **- Problem** : Lots of traps (context switches) and VMM intercepts/translations, subpar performance.

## (New) Harware solution - Nested Page Tables (NPT/EPT)
 - To avoid expensive `traps` and SPT updates,
 - Guest PTs are kept untouched,
 - VMM sets up **nested PTs** (in host RAM):
  ``GPA`` → ``HPA``,
 - CPU no longer ``traps`` on priviledged actions like loading CR3 with a GPA root,
 instead it recognizes that it's a **virtualised guest action**, 
 - CPU MMU walks guest PT (pointed to by guest ``CR3``) → yields a ``GPA``,
 - CPU MMU then walks the nested PT (pointed to by EPTP/NPTP register) → yields an ``HPA``,
 - Final access goes to HPA in real RAM

 🔹 **Advantages**
- Fast: no traps on guest PT updates.
- Guest OS can run unmodified, freely updating CR3 and PTs.
- VMM only manages GPA→HPA once, no constant syncing, minial VMM involvment.

🔹 **Disadvantages**

- MMU must do a two-level walk (can be slower per access).
- Mitigated by TLB extensions that cache combined GVA→HPA translations.
