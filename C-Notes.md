# C Notes: `static`, `volatile`, and `malloc`

---

## 1. `static`

### Meaning
- Changes **storage duration** and **visibility** of variables/functions.
- Variable lives for the entire program run (like a global), but scope is restricted.

### Cases
- **Inside a function** → persists across calls, visible only inside the function.
- **At file scope** → makes global variables/functions private to that file.

### Example (inside a function)
```c
void f() {
    static int counter = 0; // initialized once, persists across calls
    counter++;
    printf("%d\n", counter);
}
```

### Example (file scope)
```c
static int hidden_global = 42; // private to this .c file
static void helper() { /* ... */ }   // private function
```

### Summary
- Inside function → “like a global, but local to this function.”
- At file scope → “like a global, but private to this file.”

---

## 2. `volatile`

### Meaning
- Tells compiler: *“this variable may change outside program control — don’t optimize it.”*
- Prevents compiler from caching the value in a register.

### Cases
- Memory-mapped I/O registers.
- Shared variables in multithreading or interrupts.

### Example (memory-mapped I/O)
```c
#define STATUS (*(volatile unsigned int*)0xFFFF0000)
while ((STATUS & 0x1) == 0) {
    // compiler reloads STATUS each time
}
```

### Example (multithreaded flag)
```c
volatile int stop = 0;

void worker() {
    while (!stop) {
        // work until stop changes in another thread
    }
}
```

### Summary
- Does **not** make operations atomic or thread-safe.
- Only forces fresh reads/writes from memory each time.

---

## 3. `malloc`

### Meaning
- Function for **dynamic memory allocation** from the heap.
- Declared in `<stdlib.h>`.

### Example
```c
#include <stdlib.h>
#include <stdio.h>

int main(void) {
    int *arr = (int*) malloc(10 * sizeof(int));
    if (!arr) {
        perror("malloc failed");
        return 1;
    }
    for (int i = 0; i < 10; i++) arr[i] = i;
    // use arr[0..9]
    free(arr);
    return 0;
}
```

### Why needed
- Normal variables/arrays must have **fixed size** known at compile time.
- `malloc` allows size to be chosen at runtime.
- Used for dynamic data structures (linked lists, trees).
- Useful for large objects (heap is bigger than stack).

### Heap vs Stack
- **Stack**: automatic, fast, small, freed when function returns.
- **Heap**: manual allocation, flexible, persists until `free`.

---

## ✅ Quick Summary

| Keyword   | Purpose                                   | Key Idea                                                                 |
|----------|--------------------------------------------|--------------------------------------------------------------------------|
| `static` | Extend lifetime, restrict visibility       | Inside function = persistent local. At file scope = private global.      |
| `volatile` | Prevent compiler optimizations           | Value may change externally → always reload/write to memory.             |
| `malloc` | Dynamic memory allocation from the heap    | Allocate at runtime, must `free` manually. Flexible but error-prone.     |
