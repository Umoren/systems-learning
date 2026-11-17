Resources:
- **Core systems:** [CMU 15-213 — Introduction to Computer Systems](https://www.cs.cmu.edu/~213/?utm_source=chatgpt.com) · [Operating Systems: Three Easy Pieces (OSTEP)](https://pages.cs.wisc.edu/~remzi/OSTEP/?utm_source=chatgpt.com) · [MIT 6.1810 / xv6](https://pdos.csail.mit.edu/6.S081/2026/schedule.html?utm_source=chatgpt.com)
- **C:** [Beej's Guide to C](https://beej.us/guide/bgc/?utm_source=chatgpt.com)
- **Networking:** [Beej's Guide to Network Programming](https://beej.us/guide/bgnet/?utm_source=chatgpt.com) · [Beej's Guide to Networking Concepts](https://beej.us/guide/bgnet0/?utm_source=chatgpt.com)
- **Performance / hardware:** [Computer, Enhance! — Performance-Aware Programming](https://www.computerenhance.com/p/table-of-contents?utm_source=chatgpt.com)
- **Compilers / runtimes:** [Crafting Interpreters](https://craftinginterpreters.com/?utm_source=chatgpt.com)
- **Browsers:** [Web Browser Engineering](https://browser.engineering/?utm_source=chatgpt.com)
- **Storage / databases:** [SQLite Architecture](https://www.sqlite.org/arch.html?utm_source=chatgpt.com) · [TigerBeetle Documentation](https://docs.tigerbeetle.com/?utm_source=chatgpt.com)
- **Real-world systems engineering:** [Oxide RFDs](https://rfd.shared.oxide.computer/?utm_source=chatgpt.com) · [Cloudflare Engineering Blog](https://blog.cloudflare.com/tag/engineering/?utm_source=chatgpt.com) · [Fly.io Blog](https://fly.io/blog/?utm_source=chatgpt.com)

## Why we are doing this

Our goal is broader than learning C, Rust, operating systems, or any individual technology.

We want to become engineers who can reason further down the stack: from application code into runtimes, processes, memory, operating systems, storage, networks, hardware behaviour, and distributed infrastructure.

The career value comes from that depth. We should eventually be able to look at a slow service, database, runtime, proxy, browser, or distributed system and ask better questions about what the machine is doing underneath the abstraction.

Every stage of this track should therefore produce three things:

1. **Understanding** — we can explain the underlying mechanism.
    
2. **Evidence** — we have built, measured, broken, debugged, or modified something.
    
3. **Signal** — another engineer can inspect the code, experiment, benchmark, article, or talk and see what we learned.
    

---

# The shared systems foundation

The systems landscape is easier to understand as layers.

## 1. Program execution

Questions:

- What happens between source code and a running program?
    
- What does the compiler produce?
    
- What are instructions and registers?
    
- What does a function call become?
    
- What are linking and loading?
    
- Why does data layout affect performance?
    

Concepts:

```
source
  ↓
compiler
  ↓
assembly / machine code
  ↓
linker
  ↓
executable
  ↓
loader
  ↓
CPU executes instructions
```

This is one reason CMU's Introduction to Computer Systems is such a useful reference point. The course deliberately gives programmers a view of machine-level code, performance, memory, networking and concurrency rather than treating systems as an isolated OS topic.

---

## 2. Memory

Questions:

- What is an address?
    
- What is a pointer?
    
- What lives on the stack?
    
- What lives on the heap?
    
- How does allocation work?
    
- What is virtual memory?
    
- Why do caches matter?
    
- What causes a segmentation fault?
    
- What does a garbage collector save us from?
    

This is one of the places where our JavaScript background becomes useful.

We already understand objects, closures, arrays, garbage collection and references from above. Systems programming lets us inspect what those abstractions eventually require underneath.

---

## 3. The operating-system interface

This is where programs meet the OS.

Core ideas include:

```
processes
system calls
files
file descriptors
pipes
signals
process creation
IPC
permissions
virtual address spaces
```

MIT's current xv6 course starts almost immediately with Unix utilities, file descriptors, C, system calls, page tables and locking. Students eventually read the whole xv6 kernel rather than treating the OS as a black box.

Stanford's systems course similarly organizes much of its practical work around filesystems, multiprocessing, signals, multithreading and networking.

---

## 4. Concurrency

Once several things happen at once, a different class of problems appears.

We need to understand:

```
processes vs threads
shared memory
races
locks
condition variables
semaphores
atomics
deadlocks
scheduling
memory ordering
```

OSTEP gives us an excellent conceptual structure for operating systems:

**Virtualization → Concurrency → Persistence.**

Concurrency sits in the middle because the machine must safely coordinate multiple things competing for resources and shared state.

This will also deepen concepts we've already encountered in Node.js, async programming and our multi-agent orchestrator.

---

## 5. Persistence and storage

Memory disappears when the process or machine dies. Systems that matter need state to survive.

Questions include:

- What does `write()` really guarantee?
    
- When is data actually durable?
    
- What does `fsync()` do?
    
- How do filesystems organize data?
    
- Why do databases use pages?
    
- What is a write-ahead log?
    
- What are B-trees and LSM trees?
    
- What happens if power disappears halfway through a write?
    

SQLite is a particularly useful real system to study because its architecture exposes B-trees, page caches and a stable on-disk format without requiring us to understand a massive distributed database first.

TigerBeetle takes these questions much further into replication, write-ahead logging, storage faults, deterministic simulation and durability.

---

## 6. Networking

Eventually our processes need to communicate with processes on other machines.

The progression is roughly:

```
bytes
↓
sockets
↓
TCP / UDP
↓
IP
↓
DNS
↓
TLS
↓
HTTP
↓
proxies / load balancers
↓
distributed systems
```

Beej's Guide to Network Programming remains useful precisely because it works directly with the sockets interface instead of beginning at HTTP abstractions.

This area has obvious leverage for us because we've already spent years building software _on top of_ HTTP.

---

## 7. Performance and observability

Systems engineers need to answer:

> Where did the time, CPU, memory or I/O actually go?

That means learning to measure rather than guess.

Eventually our toolbox should include things such as:

```
debuggers
sanitizers
strace
profilers
flamegraphs
perf
benchmarks
assembly inspection
memory measurement
system metrics
```

Casey Muratori's Performance-Aware Programming material is explicitly designed for experienced programmers who know how to write programs but haven't learned how hardware executes them.

The industry relevance is easy to see. Cloudflare recently described both redesigning memory structures to recover more than 100 TB of RAM fleet-wide and using a Rust rewrite of its request-handling layer to better exploit new server hardware.

---

# Where the foundation branches

After the shared foundation, systems programming opens into several substantial fields.

|Branch|What becomes important|Real systems to watch|
|---|---|---|
|**Operating systems / kernels**|syscalls, schedulers, page tables, interrupts, drivers|Linux, xv6, Oxide|
|**Networking / infrastructure**|sockets, protocols, proxies, routing, load balancing, async I/O|Cloudflare, Fly.io|
|**Storage / databases**|pages, WALs, indexes, crash recovery, replication|SQLite, Postgres, TigerBeetle|
|**Compilers / runtimes**|parsing, bytecode, VMs, GC, JITs, language implementation|V8, LLVM, Rust, JVM|
|**Performance engineering**|CPU architecture, caches, profiling, allocation, vectorization|Cloudflare, databases, game engines|
|**Browsers**|networking, parsing, rendering, scheduling, VMs, sandboxing|Chromium, Firefox, WebKit|
|**Distributed systems**|state machines, replication, consensus, failure models, consistency|Fly.io, TigerBeetle, distributed databases|
|**Embedded / hardware-facing systems**|interrupts, firmware, constrained memory, devices|Oxide, automotive, robotics, IoT|

These branches overlap heavily. A database engineer needs concurrency, networking, storage and performance. A browser engineer touches compilers, networking, graphics, scheduling and security. An infrastructure engineer may move from Linux processes all the way to distributed routing state.

Berkeley's current CS162 syllabus reflects that breadth: processes, synchronization, virtual memory, storage, networking, distributed systems, reliability, security and cloud infrastructure all sit within the same systems education.

---

# The branches that are particularly interesting for us

## Networking and infrastructure

This has strong career leverage.

Fly.io's current networking work spans Rust, TCP, HTTP/2, WireGuard, global routing, load balancing, distributed state and invariants across machines. Their description of the work is almost a catalogue of systems topics.

Cloudflare provides another unusually public window into this world: Rust proxies, graceful process restarts, memory layout, caching, network measurement and hardware/software performance.

This is a serious candidate for one of our eventual specialties.

---

## Runtimes and compilers

This has a particularly interesting connection to our JavaScript background.

Instead of asking:

> How do I write JavaScript?

we can eventually ask:

> How does JavaScript run?

That opens:

```
parser
AST
bytecode
interpreter
virtual machine
garbage collector
JIT compiler
runtime
```

Robert Nystrom's _Crafting Interpreters_ eventually implements a bytecode VM in C, including values, hash tables, closures, garbage collection and optimization.

Going from nearly a decade of JavaScript to implementing a small language runtime ourselves would be a very meaningful full-circle project.

---

## Browser systems

I don't want us to overlook this because of our web-development background.

A browser is an enormous systems program.

It contains networking, parsing, rendering, task scheduling, threads, a language runtime, caching, graphics and security boundaries.

_Web Browser Engineering_ teaches those systems by implementing a small browser and explicitly positions browsers alongside operating systems, databases and compilers as systems worth understanding from the inside.

This could eventually become a uniquely strong intersection between existing web expertise and new systems knowledge.

---

## Storage and databases

Storage gives us some of the richest problems in systems engineering:

```
What happens if the machine crashes here?

What does "committed" mean?

Which bytes reached the disk?

How do we recover?

How do multiple replicas agree?

How do we make this fast without breaking correctness?
```

TigerBeetle is especially worth following because its engineering culture exposes these questions publicly: storage faults, deterministic simulation, replication, WALs, LSM structures and explicit invariants.

---

## OS / kernel work

We absolutely need operating-system knowledge.

I don't yet think we need to decide that **kernel engineering is our destination**.

xv6 gives us a controlled environment for learning page tables, system calls, locks, scheduling, filesystems and device interaction.

Oxide gives us the other end of that spectrum: real production systems involving hypervisors, host operating systems, networking, firmware and hardware/software interfaces. Their public RFDs make them an unusually valuable company to study.

---

# Distributed systems comes later

We should resist jumping immediately into Raft, consensus algorithms and distributed databases.

Distributed systems make more sense once a **single machine** is less mysterious.

Before asking how five machines coordinate durable state, we should understand:

```
how one process executes
how one machine stores data
how threads coordinate
how sockets communicate
how crashes affect state
how the OS schedules work
```

Then replication and consensus stop being diagrams and start being consequences of mechanisms we already understand.

---

# Our learning method

For this track, reading alone is insufficient.

Our recurring cycle should be:

```
UNDERSTAND
What mechanism are we studying?

        ↓

BUILD
Implement the smallest thing that exposes it.

        ↓

INSPECT
Use debuggers, traces, disassembly and system tools.

        ↓

BREAK
Introduce races, crashes, bad pointers, partial writes, failures.

        ↓

MEASURE
Observe memory, latency, throughput, CPU or I/O.

        ↓

EXPLAIN
Write or teach what happened and why.
```

The explanation stage matters because we are already strong communicators. Writing should become part of the learning instrument rather than something added after the project.

---

# How languages fit

Languages are tools inside this map.

### C

Use C to expose memory, pointers, layout, Unix interfaces and the machine model.

### Rust

Use Rust once we understand enough of those problems to appreciate what ownership, borrowing, lifetimes and safe abstractions are protecting us from.

### Zig

Worth studying later, particularly around explicit control, allocators and projects such as TigerBeetle.

### JavaScript / TypeScript

Do not abandon them.

They become comparison points:

> What does Node hide here?

> What does V8 do for us?

> Why does this JavaScript API have this shape?

That comparison may become one of the most valuable parts of the entire journey.

---

# Reference curriculum

We do not need to follow one university course verbatim.

Instead:

**CMU 15-213** gives us the programmer's view of the machine.

**OSTEP** gives us the conceptual OS spine: virtualization, concurrency and persistence.

**MIT 6.1810 / xv6** gives us a small operating system we can inspect and modify.

**Stanford CS110** provides excellent project-shaped coverage of Unix programming.

**Berkeley CS162** shows how the local-machine concepts expand into networking, storage and distributed systems.

We will borrow the strongest parts of each rather than pretend we're enrolled in university again.

---

# Industry reference systems

These are worth following throughout the track:

**Cloudflare** — networking, proxies, Rust, performance, caching and global infrastructure.

**Oxide Computer** — operating systems, hypervisors, firmware, networking, hardware/software co-design and engineering culture.

**Fly.io** — networking, global infrastructure, distributed state and Rust.

**TigerBeetle** — storage, databases, correctness, deterministic testing, Zig and distributed systems.

**SQLite** — compact, understandable production database internals.

**V8 / Chromium** — runtimes, browsers, garbage collection, JIT compilation and performance.

The goal isn't to copy these companies.

They are our reality checks for questions like:

> Does anyone solve this problem professionally?

> What does the toy version leave out?

> What engineering constraints appear at production scale?

---

# Current strategic position

We are currently at the entrance to the shared foundation.

We should **not specialize yet**.

The first phase should give us enough exposure to:

```
C
program execution
memory
Unix
processes
files
system calls
debugging
basic concurrency
networking
performance measurement
```

Then we reassess.

At that point we'll be choosing specializations from experience instead of choosing from descriptions.

The strongest potential intersections with our existing experience currently appear to be:

**networking/infrastructure**, **runtimes**, and **browser systems**.

Storage/databases may become another major direction once we have worked with persistence directly.

The point of the first phase is to discover which problems make us want to keep digging.