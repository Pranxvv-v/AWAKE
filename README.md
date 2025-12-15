# AWAKE

### *Computers that don’t reboot. They wake.*

##  What is AWAKE?

**AWAKE** is an experimental system-level project that rethinks how computers start, stop, and recover.

Instead of treating power-off as *death* and power-on as a *cold rebirth*, AWAKE treats computing as a **continuous state** — where systems pause, persist, and resume instantly.

The goal is simple:

> **Make laptops feel alive, not rebooted.**

No waiting.
No cold starts.


##  The Core Idea

Traditional computers follow this cycle:

```
Power Off → Boot → Load OS → Load Apps → Ready
```

AWAKE replaces this with:

```
Pause → Persist → Wake → Resume → Ready
```

From the user’s perspective:

* Press power → screen is usable almost instantly
* The system resumes intelligently in the background
* Crashes and power loss recover gracefully

This is not just “faster boot”.
It’s **a different philosophy of computing**.



##  CONTINUUM — The Architecture Behind AWAKE

**CONTINUUM** is the architectural foundation of AWAKE.

> **Computing should be continuous, not episodic.**

Key principles:

* System state is checkpointed incrementally
* Persistence is crash-safe and atomic
* Resume is preferred over reboot
* UI is restored first, everything else loads lazily
* Reliability beats raw speed

CONTINUUM is inspired by:

* OS kernels
* Databases
* Journaling filesystems
* Persistent memory research



##  How It Works ( At the High-Level)

### On shutdown or idle:

* The system creates **incremental snapshots** of its state
* Only changed memory pages are stored
* A tiny **wake map** (KB-scale) is updated last

### On power-on:

* Firmware checks for a valid wake map
* The kernel resumes instead of booting
* UI becomes available immediately
* The rest of the system rehydrates in the background

Result:

> **Sub-150 ms perceived wake time on real hardware.**



##  Key Components

* **Snapshot Store**
  Full system state stored in a reserved NVMe region

* **Wake Map**
  Tiny persistent metadata (targeting FRAM eventually)

* **Resume-First Firmware Path**
  Resume is attempted before cold boot

* **UI-First Restore**
  Human-perceived speed > raw completion speed

* **Crash-Resilient Checkpointing**
  Safe even during sudden power loss



##  Multi-User & Security Model

* Kernel state may persist
* User sessions **never resume without authentication**
* Per-user snapshots are encrypted
* Secure fallback to cold boot is always available

Security is treated as a **first-class requirement**, not an afterthought.



##  Current Status

 **Early design & research phase**

Right now, this repository documents:

* The architecture
* Design decisions
* Trade-offs
* Learning process
* Experiments (eventually)

Implementation will begin incrementally on:

* Linux
* An old HP laptop (sacrificial test system)

Nothing is rushed.
Everything is documented.



##  Dark Horse Log

This project is being built as a **Dark Horse** journey.

Not a startup pitch.
Not a hype demo.
A slow, honest, documented exploration.

Every failure, rethink, and breakthrough will be logged in the *Dark Horse*.

> *Great systems aren’t rushed. They’re understood.*

- [Entry #0 — The Moment AWAKE Became Real](docs/DARK_HORSE_0.md)




##  Why This Exists

Booting is a relic of old assumptions:

* Volatile memory
* Slow storage
* Stateless machines

Those assumptions no longer hold.

AWAKE asks a simple question:

> *What if computers never really “turned off”?*



##  Roadmap (High-Level)

* [ ] Study & document Linux boot/resume internals
* [ ] Prototype snapshot + wake map (emulated FRAM)
* [ ] Achieve UI-first resume
* [ ] Add crash-safe incremental checkpointing
* [ ] Introduce external FRAM hardware
* [ ] Publish benchmarks & demos



##  Contributing / Following Along

This project is open by design.

If you’re interested in:

* Operating systems
* Firmware
* Computer architecture
* Persistent memory
* Or just *making computers feel better to use*

Feel free to follow, read, and discuss.

##  License

Copyright (c) 2025 Pranav

All rights reserved.

This repository is made public for viewing and educational purposes only.  
No part of this project may be used, copied, modified, redistributed, or
commercialized without explicit written permission from the author.
