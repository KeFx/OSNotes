## 1. Processes
- A **process** is a running program.
- Each process has:
  - Its **own memory space** (code, data, stack, heap).
  - **Resources** like open files, network sockets, etc.
  - An **execution context** (program counter, registers).
- Analogy: A process is like an **office building** where one company works — walls keep it isolated from others.

---

## 2. Threads
- A **thread** is a *path of execution* within a process.
- Each thread has:
  - Its own **execution context** (program counter, registers, its own stack).
- Threads share:
  - The **process memory space**.
  - The **resources** (files, sockets) of the process.
- Analogy: Threads are **workers inside the same office**. Each has their own desk (stack), but they all share the office and common tools (memory, files).

---

## 3. Processes vs Threads
| Aspect        | Process | Thread |
|---------------|---------|--------|
| **Isolation** | Fully isolated from other processes | Shares memory/resources with other threads in same process |
| **Overhead**  | Heavy (new memory space, resources) | Light (only new execution context) |
| **Failure**   | Crash usually contained in one process | Bug can affect all threads in the process |
| **Scheduling**| OS schedules processes independently | OS schedules threads independently (like mini-processes) |

---

## 4. Why Threads are called *Lightweight Processes*
- Both **processes and threads** are schedulable units of execution.
- A thread is “lighter” because it:
  - Doesn’t need its own full memory space/resources (shares with process).
  - Only needs its own execution context (stack + registers).
- Threads cannot exist without a process, but they behave like smaller, cheaper processes inside it.

---

## 5. Key Analogies
- **Computer = city**.
- **Process = company office building** (isolated).  
- **Thread = workers inside the building** (share the office, work in parallel).

---

## TL;DR
- **Process** = a running program with its own isolated workspace.  
- **Thread** = a unit of execution inside a process.  
- Processes provide **isolation**; threads provide **parallelism** within a process.  

