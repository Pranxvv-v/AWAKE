# Dark Horse — Entry #2
### Where Boot Time Is Really Spent

**Date:** 16 December 2025  
**Focus:** Measurement and breakdown, not redesign

Boot time is often treated as a single number.

“Boots in 12 seconds.”  
“Boots in 5 seconds.”  

But boot time is not one thing.  
It is the sum of many distinct phases — some unavoidable, some historical, some purely accidental.

This entry breaks down **where time is actually spent** between pressing the power button and seeing a usable system.

## 1. The First Milliseconds: Physics Tax

These steps happen before software even exists.

### Includes:
- Power controller wake-up
- Power rail sequencing
- Voltage stabilization
- Clock generation
- CPU reset release

**Typical cost:**  
~10–50 ms

This time is dictated by:
- electrical safety
- signal stability
- hardware constraints

It cannot be eliminated.  
It can only be accepted.

## 2. Firmware Initialization: Necessary but Bloated

After hardware is electrically alive, firmware takes over.

### Firmware responsibilities:
- CPU initialization
- RAM training
- Hardware enumeration
- Security checks
- Device compatibility handling

**Typical cost:**  
~50–300 ms (sometimes more)

Important observation:
- RAM training alone can consume a large fraction of this time
- Much firmware logic exists to support *unknown* or *legacy* hardware

Not all of this work is strictly required on every power-on.

## 3. Bootloader Time: Small but Sequential

The bootloader stage is usually short, but completely blocking.

### What happens:
- Locate kernel
- Load kernel into memory
- Prepare handoff
- Transfer execution

**Typical cost:**  
~10–100 ms

This stage is simple, but rigid:
- It assumes a clean slate
- It always loads, never resumes
- It blocks until complete

## 4. Kernel Initialization: Rebuilding What Already Existed

Once the kernel starts, time usage increases sharply.

### Kernel work includes:
- Memory subsystem initialization
- Scheduler setup
- Driver initialization
- Filesystem mounting
- Service startup

**Typical cost:**  
~300 ms to several seconds

Key observation:
> The kernel rebuilds internal state every boot, even if nothing changed since the last shutdown. This is one of the largest contributors to boot time.

## 5. Userspace Startup: Where Humans Actually Wait

From a human perspective, this is where “waiting” begins.

### Includes:
- Login manager startup
- Desktop environment
- Window manager
- Background services
- User applications

**Typical cost:**  
~1–5 seconds (or more)

Important distinction:
- The system may be “booted”
- But it is not yet *usable*

Most of this time is spent:
- starting processes
- reloading state
- recreating UI elements

## 6. The Critical Gap: Booted vs Usable

A key insight emerges here.

There is a difference between:
- **System boot completion**
- **Human-perceived readiness**

The user does not care when:
- the last service starts
- the final [daemon](https://github.com/Pranxvv-v/AWAKE/blob/7df364f1e20b1b61639049fea3611b6cf7259385/docs/Dark_Horse/Explain/Daemon.md) initializes

The user cares when:
- the screen lights up
- input works
- the interface responds

Yet traditional boot treats **everything as equally urgent**.

## 7. Time Distribution (Conceptual)

Roughly speaking:

- Physics & power: unavoidable, small
- Firmware & training: unavoidable, moderate
- Kernel rebuild: large, repetitive
- Userspace rebuild: very large, mostly redundant

The majority of boot time is spent **reconstructing state that already existed before shutdown**.

## 8. Key Observation

Modern systems are not slow because hardware is slow.

They are slow because:
- state is discarded
- continuity is broken
- reconstruction is prioritized over reuse

Boot time is dominated not by physics, but by **forgetfulness**.

## 9. Why This Matters for AWAKE

AWAKE does not attempt to:
- eliminate hardware initialization
- bypass safety checks
- skip RAM training

It focuses on everything **after** that point.

This entry identifies the target:
- repeated kernel reconstruction
- repeated userspace startup
- delayed UI availability

These are design choices, not laws.

## Closing Note

Boot time feels long not because computers are slow,
but because they insist on starting over.

Understanding where time is spent makes it possible to decide:
- what must stay
- what can move
- what can disappear entirely

Redesign begins only after this map is complete.
