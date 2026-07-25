# 📘 GATE CS — Operating Systems
## Chapter 1: Introduction to OS — Revision Notes

> **Status:** ✅ Chapter 1 Complete → 🔜 Chapter 2 (Types of OS)
> **Subject:** Operating Systems | **GATE Weightage:** 6–8 marks | **Importance:** 🔥🔥🔥🔥🔥🔥🔥🔥 (8/10)

---

## 📑 Table of Contents
- [What is an OS](#-what-is-an-os)
- [Core OS Functions](#-core-os-functions)
- [OS as Resource Manager](#-os-as-resource-manager)
- [OS as Extended Machine](#-os-as-extended-machine--abstraction)
- [Kernel](#-kernel)
- [Dual Mode Operation](#-dual-mode-operation--high-priority)
- [System Calls](#-system-calls)
- [Interrupts vs Traps vs System Calls](#-interrupts-vs-traps-vs-system-calls)
- [Bootstrapping](#-bootstrapping)
- [Bridge to Chapter 2](#-why-chapter-2-builds-on-this)
- [⭐ Highest-Yield Fact](#-highest-yield-fact)

---

## 🖥 What is an OS
- Resource manager **+** abstraction layer between hardware and user/applications
- Sits between hardware and application software; controls all execution

---

## ⚙️ Core OS Functions

| # | Function |
|---|---|
| 1 | Process management |
| 2 | Memory management |
| 3 | File system management |
| 4 | I/O / device management |
| 5 | Security & protection |
| 6 | User interface (CLI/GUI) |

---

## 🧩 OS as Resource Manager
- Allocates CPU, memory, I/O devices among competing processes
- Ensures **fair**, **efficient**, and **safe** sharing of resources

---

## 🔌 OS as Extended Machine / Abstraction
- Hides hardware complexity
- Gives programs a simple, uniform interface (**system calls**) instead of raw hardware access

---

## 🧠 Kernel
- Core part of the OS — **always resident in memory**
- Has the **highest privilege**; directly manages hardware

---

## 🔐 Dual Mode Operation — *High Priority*

| Mode | Privilege | Purpose |
|---|---|---|
| **User Mode** | Restricted | Normal application execution |
| **Kernel Mode** | Unrestricted, full hardware access | OS code execution |

- A **mode bit** distinguishes the two
- Switching **user → kernel** happens via: **system calls**, **interrupts**, or **traps**

> ⭐ This is the single most GATE-relevant concept in Chapter 1 — see [Highest-Yield Fact](#-highest-yield-fact)

---

## 📞 System Calls
- Interface for user programs to request OS services (e.g., file I/O, process creation)
- Triggers a switch from **user mode → kernel mode**

---

## ⚡ Interrupts vs Traps vs System Calls

| Type | Trigger | Example |
|---|---|---|
| **Interrupt** | Hardware-generated signal | I/O completion |
| **Trap (Exception)** | Software-generated, usually an error | Divide by zero |
| **System Call** | Deliberate software request | Program requesting OS service |

---

## 🥾 Bootstrapping
- **Bootstrap loader** (in ROM/firmware) loads the OS kernel into memory at startup

---

## 🔗 Why Chapter 2 Builds on This
Chapter 2 covers how the OS handles **multiple programs/users** (the basis of multiprogramming), which directly depends on:
- The **process concept**
- **Dual-mode operation** learned in this chapter

---

## ⭐ Highest-Yield Fact
> **User Mode ↔ Kernel Mode switching** (via system calls / traps / interrupts)
> This mechanism reappears repeatedly as a foundational concept in **scheduling**, **synchronization**, and **memory management** questions later in the syllabus.

---

## ✅ Next Up
**Chapter 2 — Types of Operating Systems**

---
<sub>📚 GATE 2027 CS/IT Prep — Operating Systems Track</sub>
