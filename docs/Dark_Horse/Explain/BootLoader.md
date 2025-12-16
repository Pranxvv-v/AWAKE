## Bootloader 

> **A bootloader is a tiny program whose only job is to load the operating system kernel into memory and hand control to it.**

## Why does a bootloader even exist?

After firmware (UEFI/BIOS) finishes its job, the system is in this awkward state:

* CPU ✅ alive
* RAM ✅ usable
* Storage ✅ accessible
* OS ❌ not running

Firmware **does not run operating systems**.
So something has to bridge the gap.

That “something” is the **bootloader**.

## The boot chain (super important)

Here’s the real flow, simplified:

```
Power button
  ↓
Power controller
  ↓
Firmware (UEFI / BIOS)
  ↓
Bootloader
  ↓
OS Kernel
  ↓
Userspace (apps, UI)
```

The bootloader sits **right in the middle**.

## What does a bootloader actually do?

Very little by design.

Typical bootloader tasks:

* Locate the OS kernel on disk
* Load it into RAM
* Set up basic parameters (memory map, CPU mode)
* Jump to the kernel’s entry point

Then it’s done.
It disappears forever.

Think of it like:

> Opening the door, pointing at the stage, and leaving.

## Examples you might’ve heard of

* **GRUB** (Linux)
* **systemd-boot**
* **Windows Boot Manager**
* **U-Boot** (embedded systems)

Different names, same job.

## Why bootloaders exist separately from firmware

Firmware:

* Handles hardware
* Must be generic
* Must support many OSes

Bootloader:

* Is OS-aware
* Knows *which* kernel to load
* Knows *how* to load it

Splitting these roles keeps systems flexible.

## Why bootloaders matter for AWAKE (this is key)

Here’s the big insight:

> **Bootloaders assume a clean slate.**

They assume:

* RAM is empty
* OS is not running
* Kernel must start from scratch

That assumption is exactly what AWAKE challenges.

In AWAKE:

* We don’t want “load kernel”
* We want **resume kernel**

So the bootloader’s role shrinks or changes into:

> “Check if a resumable state exists.
> If yes → resume.
> If no → boot normally.”

Bootloader becomes a **decision point**, not a launcher.

## Why phones feel faster than PCs (tie-in)

Phones often:

* Skip traditional bootloaders
* Resume directly from saved state
* Use tightly integrated firmware

PCs keep bootloaders for flexibility and legacy.

AWAKE aims to keep flexibility **but remove unnecessary repetition**.

## One-line intuition (lock this in)

> **The bootloader is the handoff between firmware and the OS.**
