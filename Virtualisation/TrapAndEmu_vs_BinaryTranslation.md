## Common ground (Trap-and-Emulate & Binary Translation)

**In both methods:**

- Privileged instructions cannot run directly on hardware (for security/isolation).

- The VMM steps in to emulate the effect of that instruction on virtual hardware.

- The guest OS is fooled into thinking the instruction succeeded normally.

## Key differences:
 - ### Trap and Emulate:
    - **Reactive:** CPU hits priviledged instruction in user mode -> error -> `trap`
    - VMM takes over, `emulates`
    - simple to implement, but incurs heavy VM exits (context switches)

- ### Binary translation:
     - **Proactive:** VMM scans VM instructions ahead of time, identifies  privileged instructions
    - VMM translates and replaces privileged instructions with safe host instructions
    - Guest runs this modified code directly → avoids most traps
    - VMM emulates the required output for Guest
    - Much smoother performance, but requires complex translation and caching.

---
Both achieve the same illusion (“guest OS thinks it has full control”).\
Binary translation just avoids the cost of constant traps by being one step ahead.