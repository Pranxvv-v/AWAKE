# Dark Horse — Entry #1
### How Computers Actually Wake Up

**Date:** 16 December 2025  
**Focus:** Observation, not redesign

Before changing how computers wake up, it’s necessary to understand how they **actually do it today**.

This entry documents the real, unavoidable steps that occur when a modern laptop or desktop is powered on.

## 1. The  Magic of Power Button

Pressing the power button does not “start the OS”.

It does three basic things:
1. Signals the [power controller](docs/Dark Horse/Explain/Power Controller.md)
2. Enables [power rails]()
3. Releases the CPU from reset

At this point:
- No OS exists
- No RAM is usable
- No storage is accessible

The system is electrically alive, but logically empty.

## 2. Firmware Takes Control (UEFI / BIOS)

Once the CPU is released from reset, execution begins at a **fixed hardware-defined address**.

This is firmware territory.

Firmware responsibilities include:
- CPU initialization
- DRAM training and timing calibration
- Power management setup
- Basic device discovery
- Security verification (Secure Boot, signatures)

This phase is **mandatory**.
No OS can skip it.

However, much of it exists to support:
- cold hardware
- unknown configurations
- backward compatibility

## 3. Memory Must Be Rebuilt Every Time

RAM is volatile.

Because of that:
- Memory contents are assumed invalid at power-on
- DRAM must be trained and reinitialized
- All previous state is discarded

This is one of the oldest assumptions in computing:
> Power loss equals total memory loss.

This assumption heavily influences everything that follows.

## 4. The Bootloader Stage

After firmware finishes:
- Control is handed to a bootloader
- The bootloader locates the OS kernel
- The kernel is loaded into memory
- Execution jumps into kernel space

At this stage:
- The system still has **no memory of previous execution**
- The kernel assumes a clean slate

## 5. Kernel Initialization

The kernel now:
- Initializes memory management
- Sets up scheduling
- Initializes drivers
- Mounts the root filesystem
- Starts system services

This work is repeated **every single boot**, regardless of whether:
- The same OS ran minutes ago
- The same applications were used
- Nothing actually changed

Boot is treated as a full rebirth.

## 6. Userspace Finally Appears

Only after all previous stages complete:
- Login managers start
- Desktop environments load
- User sessions begin
- Applications are launched again

From a human perspective:
> The system is “ready” only at the very end.

Yet most of this time is spent **rebuilding what already existed before shutdown**.

## 7. What Is Actually Unavoidable

Some steps truly cannot be skipped:

- Power stabilization
- CPU reset handling
- Memory training
- Security verification

These are rooted in physics and safety.

## 8. What Is Assumed — Not Required

Other steps exist mainly because of **historical assumptions**:

- That memory must always start empty
- That state cannot survive power loss
- That boot must always precede usability
- That UI must wait for full system readiness

These assumptions are not laws of nature.
They are design choices.

## 9. Key Observation

Modern systems are fast enough that boot feels tolerable.

But they are also **stateful enough** that boot is increasingly wasteful.

The system already knows:
- What ran before
- What memory was used
- What devices were active
- What the user was doing

Yet none of that knowledge is reused.

## 10. Why This Matters for AWAKE

AWAKE does not aim to eliminate unavoidable hardware steps.

It questions everything **after** that.

This entry establishes the baseline:
- What must happen
- What always happens
- What happens only because “it always has”

Only after understanding this baseline does redesign make sense.

## Closing Note

This entry intentionally avoids proposing solutions.

Understanding precedes invention.

Design begins only after the ritual is fully seen for what it is.
