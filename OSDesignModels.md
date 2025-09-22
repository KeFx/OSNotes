# OS Design Models – Notes

---

## 1. Monolithic Kernel

**Description**  
Kernel is one large program that includes all system-level services (process management, memory management, file systems, device drivers, networking, etc.).  
All run in kernel space, sharing one address space.  
Communication between services is direct (function calls).  

**Strengths**  
- Very fast (no IPC or context switch overhead)  
- Simple, direct communication between services  
- Mature and widely used (Linux, classic UNIX, Windows)  

**Weaknesses**  
- One bug can crash the entire OS  
- Large attack surface (less secure)  
- Harder to maintain/extend safely  
- Portability depends on careful implementation  

---

## 2. Layered OS

**Description**  
OS is divided into layers stacked on top of each other.  
Each layer can only use the services of the layer directly below it.  
Conceptual design model, rarely used in pure form.  

**Strengths**  
- Clear structure and abstraction  
- Portable: only the lower layers need to be rewritten for new hardware  
- More secure: higher layers cannot bypass lower ones  

**Weaknesses**  
- Hard to design in practice (not all services fit strict hierarchy)  
- Performance overhead (requests may pass through many layers)  
- If a lower layer fails, higher layers depending on it may also fail  

---

## 3. Microkernel

**Description**  
Kernel is kept minimal: only includes essentials (CPU scheduling, memory management, IPC, basic hardware control).  
All other services (drivers, file systems, networking) run as user-space processes.  
All inter-service communication goes through kernel IPC.  

**Strengths**  
- High reliability: a crashed service doesn’t crash the OS  
- Strong security: strict isolation between services  
- Portable: hardware-specific code is kept small  
- Flexible and modular: services can be replaced or restarted independently  

**Weaknesses**  
- Performance overhead from IPC and context switching  
- Historically slower (though modern microkernels optimize this)  
- More complex communication model  

---

## 4. Modular Kernel (Monolithic with loadable modules)

**Description**  
Base kernel provides essentials.  
Additional services (drivers, file systems, etc.) can be loaded/unloaded as modules at runtime.  
Modules run in kernel space once loaded.  

**Strengths**  
- Flexibility: add/remove features at runtime  
- Performance: modules run as kernel code, same speed as monolithic  
- Easier maintenance/extensibility than static monolithic  
- Easier debugging: can reload buggy modules, kernel can log which module caused a crash  
- Widely used in practice (Linux, Windows NT)  

**Weaknesses**  
- Still monolithic at heart → buggy module can crash the system  
- Security weaker than microkernel (modules share kernel space)  
- No enforced isolation between modules  
