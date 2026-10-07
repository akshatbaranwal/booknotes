# Chapter 1 — Trade-Offs in Data Systems Architecture

> Notes from *Designing Data-Intensive Applications* (2nd ed). Topic sections below;
> discussions merged in place.

---

## What "data-intensive" actually means   {#data-intensive}
**Status:** Clarified · **Ref:** §1 (intro) · **Date:** 2026-07-02 · **Tags:** #fundamentals

### 💡 What made it click
"Data-intensive" is a property of a **workload / component**, not a company. One system
can hold both kinds of work — OpenAI is the clean example:

```
OpenAI, split by component
─────────────────────────────────────────────
 GPU inference / training      →  COMPUTE-intensive
   billions of matrix multiplies per token,
   GPUs pinned, data (your prompt) is tiny

 Chat history / memory / auth  →  DATA-intensive
   store every conversation, index it, fetch the
   right memory fast, replicate so nothing is lost,
   scale to 100s of millions of users
─────────────────────────────────────────────
```

DDIA explicitly excludes the GPU/compute side and is entirely about the data side.

### My doubt
What does "data-intensive" actually mean? OpenAI has lots of chat history/memory (data)
but also SUPER LARGE models eating GPU — so which is it?

### Resolution
Every app burns two resources: **CPU cycles** and **data**. The label points at whichever
one is the *bottleneck that makes the problem hard*.

- **Compute-intensive** — the hard part is raw calculation (weather sim, model training,
  hashing). Data may be tiny. Fix for slowness: throw more/faster CPUs/GPUs at one box.
- **Data-intensive** — the CPU is basically bored; the hard part is the *data itself*:
  its **amount, complexity, and speed of change**. Faster processors don't help you store
  500M users' logs reliably, keep them consistent across data centers, or fetch the right
  record in 20ms.

Note CPU speed doesn't even appear in the data-intensive definition. So "is X data-intensive?"
is the wrong question — ask "*which component*?" OpenAI's token generation is compute-bound
(DDIA skips it); its chat history + memory retrieval is data-bound (DDIA's whole subject).

### One-liner to reread
Data-intensive = the hard part is the data's volume/complexity/velocity, not CPU cycles.
Litmus test: if a faster box on one machine mostly fixes it → compute-intensive; if the pain
is storing/moving/indexing/keeping-correct lots of fast-changing data across many machines →
data-intensive. It's a label on a *workload*, not a company.

### Related
_(none yet)_
