# Dark Horse — Entry #5
### The Layers of State

**Date:** 20 December 2025  
**Focus:** Structuring system state into clear, independent layers

If continuity is to be reliable, state cannot be treated as a single monolith.

Not all state is equal.
Not all state should persist.
Not all state fails the same way.

This entry defines the **layers of state** in a continuous system and establishes clear boundaries between them.

## 1. Why Layers Matter

Treating “system state” as one thing leads to fragility.

If everything resumes together:
- a single corruption poisons the whole system
- recovery becomes all-or-nothing
- trust is lost quickly

Layering allows:
- selective persistence
- partial recovery
- graceful fallback

Continuity survives only when failure is contained.

## 2. Layer 0: Hardware State

**What it is:**
- Power rails
- Clock signals
- CPU reset state
- DRAM electrical state

**Properties:**
- Controlled by physics
- Lost on full power removal
- Reconstructed on every cold power-on

**Rule:**
> Hardware state is never persisted.

This layer is always rebuilt.
It defines the hard boundary between physics and software.

## 3. Layer 1: Firmware State

**What it is:**
- Firmware configuration
- Boot policies
- Security roots
- Resume eligibility checks

**Properties:**
- Small
- Persistent
- Highly trusted

**Rule:**
> Firmware state decides *whether* continuity is allowed, not *what* resumes.

Firmware never resumes the system itself.
It only authorizes the attempt.

## 4. Layer 2: Kernel State

**What it is:**
- Kernel memory
- Scheduler state
- Core data structures
- Device driver state

**Properties:**
- Central
- Privileged
- Highly sensitive

**Rule:**
> Kernel state may persist only if it remains verifiable and trusted.

If kernel integrity cannot be proven:
- kernel state is discarded
- system falls back to boot

Kernel continuity is powerful, but dangerous if mishandled.

## 5. Layer 3: System Services State

**What it is:**
- Daemons
- Background services
- Networking stacks
- Audio, display, power services

**Properties:**
- Semi-privileged
- Restartable
- Often idempotent

**Rule:**
> System services may resume selectively.

Some services:
- resume cleanly
- others restart safely

This layer tolerates partial continuity.

## 6. Layer 4: User Session State

**What it is:**
- Logged-in users
- Desktop environments
- Session managers

**Properties:**
- Identity-bound
- Security-sensitive
- Human-facing

**Rule:**
> User state never resumes without authentication.

Continuity here is conditional.
Trust is established by the user, not the system.

## 7. Layer 5: Application State

**What it is:**
- Application memory
- UI state
- In-flight work

**Properties:**
- Large
- Unpredictable
- Least trusted

**Rule:**
> Application state is opportunistic.

If it resumes cleanly:
- great

If not:
- the system remains usable
- applications restart independently

No single application is allowed to block system readiness.

## 8. Independence Between Layers

Each layer must obey one rule:

> **Failure in a higher layer must not invalidate lower layers.**

Examples:
- App crash → does not affect kernel
- User session corruption → does not affect system services
- Service failure → does not force reboot

This asymmetry is intentional.

## 9. Resume Is Layered, Not Binary

Resume is not:
- ON or OFF

It is:
- Kernel resumes
- Services partially resume
- UI appears
- Apps hydrate lazily

Continuity happens progressively, not all at once.

## 10. Key Observation

Boot treats all layers as inseparable.

Continuity treats them as independent.

This is the structural difference between:
- restarting a machine
- waking a system

## Closing Note

Defining layers is an act of restraint.

It limits what continuity is allowed to do.
It makes failure survivable.
It makes reasoning possible.
