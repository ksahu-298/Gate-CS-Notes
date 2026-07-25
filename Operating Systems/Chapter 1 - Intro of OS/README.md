# Introduction to Operating Systems — GATE CS Revision Notes

## 1. Topic Summary
OS is a resource manager and abstraction layer between hardware and user programs, providing process/memory/file/device management. In GATE, this topic mostly feeds conceptual/definitional questions and sets the base for Process Scheduling, Memory, and File Systems.

## 2. Core Concepts
- **OS functions:** process management, memory management, file management, I/O/device management, security & protection, resource allocation.
- **Kernel:** core part of OS always resident in memory; runs in privileged (kernel) mode.
- **Dual-mode operation:** User mode (unprivileged) vs Kernel mode (privileged) — enforced via **mode bit**.
- **System calls:** interface between user programs and OS services (fork, exec, wait, read, write, open, close).
- **Interrupts vs Traps vs Exceptions:**
  - Interrupt — asynchronous, from hardware.
  - Trap/Exception — synchronous, from software (e.g., system call, divide-by-zero).
- **Types of OS:**
  - Batch, Multiprogrammed, Time-sharing, Multitasking, Multiprocessing (tightly/loosely coupled), Distributed, Real-time (Hard/Soft), Network OS.
- **Multiprogramming vs Multitasking vs Multiprocessing:**
  - Multiprogramming — multiple jobs in memory, CPU switches to maximize utilization (no time-slicing guarantee).
  - Multitasking — time-shared multiprogramming, gives illusion of parallel execution.
  - Multiprocessing — multiple CPUs, true parallel execution.
- **Bootstrap loader / Bootstrapping:** small program that loads OS kernel at startup (stored in ROM/firmware).
- **Kernel architectures:** Monolithic, Microkernel, Layered, Hybrid, Exokernel.
- **System call execution flow:** user program → trap instruction → mode switch to kernel → OS service routine → mode switch back to user.

## 3. Definitions
- **Operating System:** System software that acts as an intermediary between user and hardware, managing resources and providing services.
- **Process:** A program in execution (active entity with its own PCB, address space).
- **Kernel Mode:** Privileged CPU mode allowing execution of all instructions including I/O and memory-protection instructions.
- **User Mode:** Restricted mode; privileged instructions cause a trap to OS.
- **System Call:** Programmatic way a program requests a service from the OS kernel.
- **Interrupt Vector Table (IVT):** Table mapping interrupt numbers to their handler (ISR) addresses.
- **Context Switch:** Saving state of one process and loading state of another so CPU can switch execution.
- **Spooling:** Simultaneous Peripheral Operations On-Line — using disk as a buffer between devices with speed mismatch.
- **Multiprogramming Degree:** Number of processes resident in main memory simultaneously.

## 4. Key Formulae
Introductory OS is mostly conceptual — no direct numerical formulae here (formulae appear later in Scheduling, Memory, Disk). Still useful:
- **CPU Utilization (conceptual):** ↑ Multiprogramming degree → ↑ CPU utilization (up to a point, then thrashing risk).
- **Turnaround Time = Completion Time − Arrival Time** (used across scheduling, mentioned here as foundational).

## 5. Important Properties
- Kernel mode instructions are called **privileged instructions**; attempting them in user mode → trap to OS (illegal instruction trap).
- **Mode bit = 0** → kernel mode; **Mode bit = 1** → user mode (convention may vary by textbook, but GATE typically states it explicitly in question).
- Interrupts have **higher priority handling** than normal instruction execution; CPU checks for interrupts after each instruction cycle.
- **Dual-mode** protects OS and other programs from a misbehaving user program.
- Timer interrupt is essential for **preemptive multitasking** — prevents a process from monopolizing CPU.
- System calls are the **only** legitimate entry point into kernel mode from a user process.
- Trap instruction is a form of **software-generated interrupt**, synchronous with program execution.

## 6. Algorithms (if applicable)
Not applicable directly at intro level — conceptual flow only:
1. User program executes → needs OS service → executes system call (e.g., `INT` instruction on x86).
2. Trap raised → CPU switches to kernel mode → jumps to address in Interrupt Vector Table.
3. OS service routine executes.
4. Control returns to user program → mode switches back to user mode.

## 7. Time & Space Complexities
Not applicable — no algorithmic complexity in this sub-topic (complexities begin from Process Scheduling onward).

## 8. Comparison Table

| Aspect | Multiprogramming | Multitasking | Multiprocessing |
|---|---|---|---|
| CPUs | 1 | 1 | ≥2 |
| Goal | Maximize CPU utilization | Fast response, illusion of parallelism | True parallel execution |
| Switching | Non-preemptive typically | Preemptive (time-sliced) | Simultaneous on multiple CPUs |
| Example | Old batch systems with job pool | Modern desktop OS | Servers with multi-core/multi-CPU |

| Aspect | Interrupt | Trap (Exception) |
|---|---|---|
| Source | Hardware (external) | Software (internal, CPU-generated) |
| Sync/Async | Asynchronous | Synchronous |
| Example | I/O completion, timer | System call, divide by zero, page fault |

| Aspect | Kernel Mode | User Mode |
|---|---|---|
| Privilege | Full hardware access | Restricted |
| Mode bit | Typically 0 | Typically 1 |
| Instructions | All (privileged + non-privileged) | Non-privileged only |

## 9. PYQ Focus Areas
- Dual-mode operation & mode bit reasoning (very frequently asked as conceptual MCQ/MSQ).
- Difference between interrupt, trap, and system call (asked almost every 1-2 years).
- Multiprogramming vs multitasking vs multiprocessing distinctions.
- Identifying which operations require kernel mode (e.g., "which of the following requires OS intervention").
- Purpose of timer interrupt in preventing infinite loops/monopolization.
- Spooling vs buffering conceptual distinction.
- Statements-based MSQ: "Which of the following is/are true about OS" (mixes kernel, mode bit, system calls, interrupts).

## 10. Frequently Confused Concepts
- **Multiprogramming vs Multitasking** — multiprogramming doesn't guarantee time-sharing; multitasking does (CPU switches so fast users perceive parallelism).
- **Interrupt vs Trap** — interrupt = external/hardware, asynchronous; trap = internal/software, synchronous. System call is implemented via a trap.
- **Process vs Program** — program is passive (code on disk); process is active (in execution, has PCB).
- **Kernel vs Operating System** — kernel is the core component of OS, not the entire OS (OS includes shell, utilities, etc.).
- **Tightly coupled vs Loosely coupled multiprocessing** — tightly coupled = shared memory (true multiprocessor systems); loosely coupled = each has own memory, communicate over network (distributed systems).
- **Spooling vs Buffering** — buffering handles speed mismatch for a single job's I/O; spooling manages I/O of multiple jobs using disk as a large buffer, enabling overlap between jobs.

## 11. Common Mistakes
- Assuming mode bit convention (0/1) is universal — GATE questions specify it; don't assume.
- Confusing "system call" with "interrupt" — a system call is executed via a trap, not a hardware interrupt.
- Thinking multiprogramming automatically implies time-sharing/fairness — it does NOT.
- Forgetting that privileged instructions executing in user mode cause a **trap**, not silent failure.
- Treating "kernel" and "OS" as fully synonymous in strict definitional questions.

## 12. Memory Tricks / Mnemonics
- **"HAT"** for Interrupts: **H**ardware, **A**synchronous, **T**imer-driven.
- **"SST"** for Traps: **S**oftware-generated, **S**ynchronous, **T**riggered by current instruction (e.g., syscall, divide error).
- Remember mode transition as: **User → Trap → Kernel → Service → Return → User** ("Utility Trucks Keep Service Running Uninterrupted").

## 13. One-Minute Revision (≤20 bullets)
- OS = resource manager + abstraction layer between hardware & user.
- Kernel = core OS component, always in memory, privileged mode.
- Dual mode: User mode vs Kernel mode, enforced by mode bit.
- Privileged instructions → only in kernel mode; attempt in user mode → trap.
- System call = user request for OS service, implemented via trap instruction.
- Interrupt = hardware, asynchronous; Trap = software, synchronous.
- Timer interrupt enables preemptive multitasking, avoids CPU monopolization.
- Multiprogramming = multiple jobs in memory, CPU utilization ↑, no timesharing guarantee.
- Multitasking = time-shared multiprogramming, illusion of parallelism.
- Multiprocessing = multiple CPUs, true parallel execution (tightly vs loosely coupled).
- Spooling = uses disk as buffer for multiple jobs' I/O.
- Bootstrapping = loading kernel into memory at startup, via bootstrap loader in ROM.
- Kernel architectures: Monolithic, Microkernel, Layered, Hybrid, Exokernel.
- Process = program in execution (active); Program = passive code.
- IVT maps interrupt numbers to handler addresses.
- Context switch = saving/restoring process state during CPU switch.
- Real-time OS: Hard (strict deadlines) vs Soft (deadlines preferred, not mandatory).
- OS types: Batch → Multiprogrammed → Time-sharing → Distributed → Real-time.
- System call flow: user program → trap → mode switch → kernel routine → return.
- GATE loves statement-based MSQs mixing mode bit, interrupt/trap, and multiprogramming facts.

## 14. Expected GATE Question Types
- **1-mark MCQ:** "Which of the following is true about kernel mode?" / definitional recall.
- **MSQ (multiple select):** Statements combining interrupt/trap/system call/mode-bit facts — select all true statements.
- **Conceptual 2-mark:** Given a scenario (e.g., "a user program tries to execute an I/O instruction directly"), identify what happens (trap generated, OS intervenes).
- **Comparison-based:** Distinguish multiprogramming/multitasking/multiprocessing given a system description.
- **Cross-topic integration:** OS intro concepts combined with process states/scheduling in a single question (e.g., context switch during interrupt handling).

## 15. Interview Questions
- What is the difference between a process and a program?
- Explain the difference between kernel mode and user mode, and why dual-mode is necessary.
- What happens internally when a system call like `read()` is invoked?
- Differentiate between interrupts and traps with examples.
- What is the role of the timer interrupt in modern operating systems?
- Explain monolithic vs microkernel architecture with pros/cons.
- What is spooling and how does it differ from buffering?
- Why can't user-mode programs directly access hardware devices?
