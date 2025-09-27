# Trap-and-emulate
 - Attempting a privileged instruction in user mode causes an error -> `trap`->VM exits

 - VMM gains control, analyzes error, executes operation as attempted by guest -> `emulates`

 - VMM returns control to guest -> `return`

---
---
 **Problem:**  with multiple VMs running with their own virtual kernels -> lots of privileged calls -> lots of traps and emulates -> performance tanks

 ## Solution: Hardware support
 Modern CPUs added virtualization extensions (Intel VT-x, AMD-V, ARM VE):
- They allow the hypervisor to configure a new CPU mode (“ring –1”).

- Many guest privileged instructions can now run directly on hardware, with the CPU ensuring they don’t escape their sandbox.

- This reduces reliance on trap-and-emulate, speeding things up massively.