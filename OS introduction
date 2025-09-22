1. Multiprogramming
Problem before it: If one program blocks on I/O (e.g., waiting for disk or network), the CPU sits idle. Huge waste, since CPU is way faster than I/O.


Solution: Keep several jobs in memory at once. When one job waits for I/O, OS switches to another that’s ready to run.


Goal: Maximize CPU utilization.


User perspective: Not about interaction, just throughput. Early batch systems used this.
2. Multitasking (time-sharing)
Problem with multiprogramming: Great for throughput, but still no interactivity. If every program just waits its turn, you can’t have multiple users or interactive apps feel responsive.


Solution: Extend multiprogramming with preemptive scheduling:


Each process gets a fixed time slice (quantum).


When time’s up, the OS forcibly switches to another.


Alternation is so rapid that users feel like their programs run at the same time.


Goal: Responsiveness and fairness, not just CPU utilization.


User perspective: Feels like multiple things are running “simultaneously.”

DUAL MODE

