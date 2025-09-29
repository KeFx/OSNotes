## Memory management

### Normal OS memory management:
 - OS sets up **page table** (virtual pages -> physical frames)
 - **Page table** is just data that lives in physical memory
 - CPU has a register (CR3) that points to the root of the active page table in RAM
---
 - Program wants to access a memory address `0x00401234` (virtual)
 - CPU needs to know the physical address that maps to
 - The hardware **MMU** (Memory Management Unit inside the CPU) looks at the page table (pointer in CR3)
 - Finds mapping: Virtual `0x00401234` → Physical `0x1ABCD234`
 - CPU Loads/Stores to that physcial RAM location

 