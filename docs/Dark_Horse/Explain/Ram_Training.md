## What is RAM training?
> **RAM training is the process where the system figures out *how fast* and *when* it can safely talk to RAM after power-on.**

RAM doesn’t just work automatically.
It has to be **taught the timing** every cold start.

## Why RAM can’t “just start working”

Modern RAM (DDR4 / DDR5) is:

* insanely fast
* extremely sensitive
* operating at the edge of physics

We’re talking:

* billions of operations per second
* signals racing through copper traces
* tiny voltage margins

At these speeds:

> **a few picoseconds too early or too late = wrong data**

So the system must calibrate.

## What actually needs to be trained?

Several things

### Timing (the big one)

The CPU and RAM must agree on:

* when data is valid
* when it should be read
* when it should be written

This includes:

* [CAS latency](https://github.com/Pranxvv-v/AWAKE/blob/76fa5486b31c2abac6017fd181ca6765050ce8d4/docs/Dark_Horse/Explain/CAS_Latency.md)
* read/write delays
* command spacing

### Signal alignment

Signals don’t arrive instantly.

* Different wires have different lengths
* Temperature affects speed
* Voltage affects behavior

So the controller checks:

* “Did bit 3 arrive before bit 7?”
* “Is this line lagging?”

Then it compensates.

### Voltage stability

RAM needs:

* very specific voltage levels
* stable power

Training checks:

* Is voltage clean?
* Is it fluctuating?
* Can we run at rated speed?

### Frequency selection

If the system *can’t* reliably hit max speed:

* it slows RAM down
* prioritizes correctness over speed

That’s why:
* unstable systems boot at lower RAM speeds

## How does training actually happen?

At a high level:

1. Power rails stabilize
2. Clock starts
3. Memory controller sends test patterns
4. RAM responds
5. Controller adjusts timing
6. Repeat until reliable
7. Lock settings
8. RAM becomes usable

All of this happens:

* before the OS
* before [bootloader](https://github.com/Pranxvv-v/AWAKE/blob/f16ab91548b6137d3eb73fc8509e26495069f9b0/docs/Dark_Horse/Explain/BootLoader.md)
* inside firmware / memory controller

## How long does RAM training take?

Rough numbers (varies by system):

* Best case: **a few ms**
* Typical: **10–50 ms**
* Worst case (high-speed DDR5): **100+ ms**

This is one of the **largest unavoidable costs** of cold boot.

## Why sleep/wake is faster than cold boot

Now connect the dots 

* Sleep → RAM stays powered
* Timing is already known
* No training needed

So wake-from-sleep feels instant.

Cold boot:

* RAM lost power
* Everything forgotten
* Training must be redone

This is a **huge reason** AWAKE can’t skip *everything*.

## Why AWAKE respects RAM training

Here’s the mature insight:

> **RAM training is not waste.
> Rebuilding software state is.**

So AWAKE:

* accepts RAM training
* accepts clock startup
* accepts power sequencing

But questions:

* why rebuild kernel?
* why relaunch UI?
* why forget everything?

That’s where the win is.

## One sentence to remember (lock this in)

> **RAM training is the system re-learning how to speak to memory safely after power loss.**
