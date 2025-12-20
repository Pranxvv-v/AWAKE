## Metadata (one-line first)

> **Metadata is data *about* data.**

Not the content itself — **information that describes the content**.

## Simple real-world analogy

### A book

* **Book text** → the actual data
* **Title, author, page count, ISBN** → metadata

Metadata tells you:

* what the thing is
* how to find it
* how to trust it
* how to use it

## In computing terms

### Example: a file on your laptop

| Thing         | Data or Metadata? |
| ------------- | ----------------- |
| File contents | Data              |
| File name     | Metadata          |
| Size          | Metadata          |
| Created date  | Metadata          |
| Permissions   | Metadata          |
| Checksum      | Metadata          |

You can delete metadata without deleting the data
You can change metadata without changing the data


## Why metadata exists at all

Because raw data alone is **useless without context**.

Imagine this:

* You find 16 GB of random bits on disk
* No label
* No size info
* No integrity info

Question:
  *Is this a photo? Memory snapshot? Corrupted junk?*

You literally can’t know.

Metadata answers those questions.

## Metadata in **AWAKE / CONTINUUM** context (important)

When we say:

> “state is written before metadata”

Here’s what that actually means.

### Data (the heavy stuff)

* RAM snapshot
* Kernel state
* Process memory
* User session data

This is **big**, slow to write, and expensive.

### Metadata (the small but critical stuff)

* Snapshot ID
* Version number
* Size
* Integrity hash
* “This snapshot is complete” flag
* Pointer to where the snapshot lives

This is **tiny** (KBs), fast to write, but *decides everything*.

## Why metadata is dangerous if mishandled

If metadata lies, the system breaks.

### Bad scenario

1. Metadata says: “Snapshot complete”
2. Actual snapshot write was interrupted
3. Resume uses corrupted data
4. System crashes or corrupts state

That’s catastrophic.

## Correct rule (this is why we care)

> **Write data first.
> Write metadata last.**

That way:

* If power dies early → metadata still points to old, valid data
* Metadata never lies
* Resume is always safe

This is how:

* databases work
* filesystems work
* reliable systems work

## Metadata is the *decision-maker*

Data is the body.
Metadata is the brain.

The system doesn’t *read* all data to decide what to do — it reads metadata.

That’s why metadata must be:

* small
* accurate
* verifiable
* atomic

## One-line intuition (lock this in)

> **Metadata is what lets a system trust, locate, and reason about its data.**

Without it:

* no resume
* no safety
* no recovery
