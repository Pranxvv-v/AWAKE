## Daemon — simple definition first

> **A daemon is a background program that runs quietly and continuously to keep the system working.**

It doesn’t have a window.
It doesn’t ask for input.
It just *exists* and does its job.

## Why are they called “daemons”?

It comes from the idea of a **helper spirit** that works invisibly in the background.

So think:

> *background helper*, not demon.

## What do daemons actually do?

Examples you use every day:

*  **Network daemon** → keeps Wi-Fi working
*  **Audio daemon** → handles sound
*  **Time daemon** → keeps system time synced
*  **File daemon** → watches file changes
*  **Security daemon** → enforces permissions

They usually:

* start at boot
* run all the time
* wake up only when needed

## How daemons fit into boot time (important)

During boot:

1. Kernel starts
2. Userspace begins
3. **Daemons start one by one**
4. Each daemon:

   * loads configs
   * initializes resources
   * waits for events

This is why:

* boot *finishes*
* but the system still feels busy

Lots of daemons are starting in the background.

## Why daemons matter for AWAKE

Here’s the key insight 

> **Daemons forget everything on reboot.**

So every boot:

* network daemon reconnects
* audio daemon reinitializes
* display daemon rebuilds state
* service daemons restart from zero

AWAKE asks:

> “Why restart daemons that were already running 10 seconds ago?”

Instead of:

* stopping them
* killing their memory
* restarting them

AWAKE wants to:

* pause them
* persist their state
* resume them intelligently

That’s where *huge* time is lost today.

## One-line intuition (remember this)

> **Daemons are the invisible workers that make your system usable.**

Boot feels slow because you’re waiting for them to wake up and get ready.

## Tiny example (to lock it in)

When you boot Linux:

* NetworkManager (daemon) starts
* PulseAudio / PipeWire (daemon) starts
* systemd services (daemons) start

If they could just **resume** instead of restart:

* network would already be connected
* audio already ready
* UI instantly responsive

That’s the nitros.
