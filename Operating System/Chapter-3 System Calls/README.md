# OS Interfaces, System Calls & Mode Bit — GATE CS Notes

> Base content as provided, with GATE-exam-specific additions layered in (marked with 🎯 **GATE Angle**).

---

## 1. User Interface (UI)

The user interface is the model through which a user conveys instructions to the OS. Three primary types:

- **Command Line Interface (CLI):** Original method, users type commands. Maximum control/efficiency, steep learning curve.
- **Graphical User Interface (GUI):** Visual icons + mouse clicks. Double-clicking an icon still triggers underlying commands.
- **Touch Screen:** Most intuitive; every touch triggers an underlying command (e.g., opening a camera app).

🎯 **GATE Angle:**
- UI types themselves are **rarely directly tested** in GATE (low weightage, almost never a standalone question) — treat this as conceptual grounding only, don't over-invest time here.
- The one testable idea: **regardless of interface (CLI/GUI/Touch), the underlying execution path is identical** — user action → system call → kernel service. GATE occasionally frames a question testing whether you know that GUI/touch actions are *not* a separate execution mechanism, just a different trigger for the same system call path.
- Don't confuse **shell** (a CLI program that interprets commands, itself a user-mode process) with the **kernel** — shell is not part of the kernel; it invokes system calls like any other program.

---

## 2. System Calls

A System Call is the programmatic way a process requests a service from the Operating System kernel.

- **Purpose and Security:** OS is the ultimate controller of hardware. Direct user access to hardware (screen, disk, memory) is disallowed to prevent risk/misuse.
- **The Mechanism:** e.g., `printf` cannot control screen hardware directly — the request is passed to the OS as a system call (`write`).
- **Categories:** process control, file management, device management, information maintenance, communication. Examples: "Get Time", "Set Time".

🎯 **GATE Angle (high-yield — this is the real exam core):**
- The `printf` → `write()` example is a **classic illustration of API vs system call**: `printf()` is a C library (API) function; internally it invokes the `write()` system call. GATE has tested this API-vs-syscall distinction directly.
- **Categories are directly testable** — memorize with examples:
  | Category | Example calls |
  |---|---|
  | Process control | `fork()`, `exec()`, `exit()`, `wait()` |
  | File management | `open()`, `read()`, `write()`, `close()` |
  | Device management | `ioctl()`, `read()`/`write()` on device files |
  | Information maintenance | `getpid()`, `alarm()`, `time()` — includes "Get Time"/"Set Time" |
  | Communication | `pipe()`, `shmget()`, `send()`, `recv()` |
- **Mechanism recap (already covered in your system-call notes):** system call number loaded into register → trap instruction → mode bit flips → system call table lookup → kernel routine runs → mode bit flips back → control returns. This exact sequence is where MSQ statement-verification questions live.
- **Why not direct hardware access?** This "risk/misuse" idea is GATE's way of testing **protection and isolation** as a *reason* for dual-mode design — expect reasoning-based questions like "Why must I/O instructions be privileged?"

---

## 3. Mode Bit (Dual-Mode Operation)

- **User Mode (Bit 1):** User applications execute here; limited access to the system.
- **Kernel Mode (Bit 0):** Privileged/monitor mode; OS has full control over hardware.
- **Transition:** User process (Bit 1) triggers a system call → paused → mode bit flips 1→0 → Kernel Mode executes the privileged request → mode bit flips back 0→1 → control returns to user process.

🎯 **GATE Angle (very high-yield, appears almost every cycle):**
- ⚠️ **Mode bit convention is NOT universally fixed across textbooks** — some sources use 0 = kernel/1 = user (as given here, Silberschatz-style), but GATE questions **always state the convention explicitly** in the problem. Never assume; read the question's convention every time. This is the single most common trap in mode-bit questions.
- **Trigger for mode switch = trap**, not a hardware interrupt. A system call is a *type* of trap (software-generated, synchronous). Keep this chained: **System call → Trap → Mode bit flip → Kernel mode**.
- **Privileged instructions** (I/O instructions, setting the mode bit itself, memory management instructions like setting timer/interrupt vector) can **only execute in kernel mode**. If attempted in user mode → illegal instruction trap → OS intervenes (may terminate the process).
- Note the **subtlety**: the user process *cannot* flip the mode bit itself — only the trap/interrupt handling hardware+OS mechanism can do it. If a GATE question implies a user program directly modifies the mode bit, that statement is **false** — it violates the entire premise of protection.
- **Timer interrupt** ties in here too: it's a hardware interrupt (not a trap) that forces a mode switch back to kernel periodically, enabling preemptive scheduling — commonly cross-referenced in the same question set as mode bit basics.

---

## Frequently Confused Concepts (GATE-specific)
- **System call vs Trap vs Interrupt** — system call is the *request*; trap is the *software mechanism* that delivers it to the kernel; interrupt is a separate, *hardware-triggered* async event. All three cause a mode switch, but only interrupts are asynchronous.
- **API vs System Call** — one API call may map to 0, 1, or multiple system calls; don't assume 1:1.
- **Mode bit value convention** — always check what the question states; don't default to memorized value.

## Common Mistakes
- Assuming mode bit = 1 always means kernel mode (convention varies by source/question).
- Treating GUI/CLI/touch as having fundamentally different execution paths at the kernel level — they don't.
- Forgetting `printf()` is an API call, not itself a system call.
- Believing a user process can toggle the mode bit directly.

## One-Minute Revision
- 3 UI types (CLI/GUI/Touch) — same underlying system call mechanism regardless of interface.
- System call = programmatic request from process to kernel; only legal path into kernel mode.
- 5 system call categories: process control, file mgmt, device mgmt, info maintenance, communication.
- API (e.g., `printf`) wraps system calls (e.g., `write`) — not always 1:1.
- Mode bit distinguishes user mode vs kernel mode — convention varies, always check the question.
- Trap = software-triggered synchronous mechanism that causes the mode switch for a system call.
- Privileged instructions execute only in kernel mode; attempted in user mode → trap/error.
- User process cannot flip the mode bit itself — only OS/hardware trap mechanism can.
- Timer interrupt (hardware, async) also forces mode switch — enables preemption, distinct from traps.

## Expected GATE Question Types
- MSQ: "Which of the following statements about system calls/mode bit are true?"
- Conceptual: Given a mode-bit convention explicitly stated, trace what happens when a privileged instruction runs in user mode.
- API vs syscall identification: "Which of the following is a system call vs a library function?"
- Category matching: match a given call (e.g., `getpid`, `pipe`) to its system call category.
