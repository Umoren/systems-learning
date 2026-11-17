# Phase 1 — I type `./program`. What happens next?

## Goal

We want to understand what happens from the moment we write a small C program until the operating system runs it and the program finishes.

By the end of Phase 1, both of us should be able to explain this flow clearly:

```
source code
    ↓
compile
    ↓
link
    ↓
executable
    ↓
./program
    ↓
process
    ↓
memory + operating system
    ↓
other processes / files / input / output
    ↓
exit
```

We are learning this from the **programmer's point of view**.

We are not trying to understand kernel internals yet.

---

## Our environments

**Sammy:** Windows + WSL/Linux  
**Pius:** macOS

We will study the concept first, then compare how Linux and macOS expose it.

When something differs, we ask:

> What are both operating systems trying to accomplish, and how does each one do or show it?

We do not need to make both environments identical.

---

# Curriculum Map

|Session|Question|Build|
|---|---|---|
|**1**|What is `./program`?|`hello-systems`|
|**2**|What happens when I run it?|Extend `hello-systems`|
|**3**|Where does my program keep things?|`process-lab`|
|**4**|How does my program ask the OS to do things?|Extend `process-lab`|
|**5**|How do two programs work together?|`tinysh` — Part 1|
|**6**|Can we explain the whole journey?|`tinysh` — Complete|

---

# Week 1 — From Code to Process

```
source code
    ↓
object files
    ↓
executable
    ↓
process
```

## Session 1 — What is `./program`?

### Build

`**hello-systems**`

Create:

```
hello-systems/
├── main.c
├── greet.c
└── greet.h
```

`main.c` calls a `greet()` function defined in `greet.c`.

The finished program should print:

```
Hello from our first systems program.
```

### What we learn

We want to understand:

- what C source code is
    
- what a compiler does
    
- what an object file is
    
- what linking does
    
- what an executable is
    
- why `./hello` can be run
    

We compile the source files separately before linking them together.

We also inspect what our operating systems produced.

On Linux we will encounter **ELF** executables.

On macOS we will encounter **Mach-O** executables.

We do not need to know those formats beforehand. We encounter them first, then investigate what they mean.

### Why are we building this?

Normally, a command such as:

```
gcc hello.c -o hello
```

hides several steps.

Using multiple files lets us physically see:

```
source files
    ↓
object files
    ↓
linked executable
```

Before we understand what happens when `./program` runs, we need to know what `program` actually is.

---

## Session 2 — What happens when I run it?

### Extend `hello-systems`

The program should now print its own process ID.

Then create a small experiment where:

1. a parent process starts
    
2. it creates a child process
    
3. the child runs another program
    
4. the parent waits for the child
    
5. the parent reports how the child finished
    

We will use:

- `getpid()`
    
- `fork()`
    
- `exec()`
    
- `wait()`
    

### What we learn

The important distinction is:

```
PROGRAM
a file containing instructions

        ↓ run it

PROCESS
a running instance of that program
```

One executable can be used to create many processes.

We inspect those processes using the tools available on Linux and macOS.

### Why are we building this?

A file sitting on disk is not the same thing as a running program.

Understanding **program vs process** gives us the foundation for shells, servers, containers, background jobs and many other systems concepts.

---

# Week 2 — Inside a Running Process

```
              process
             /       \
         memory       operating system
```

## Session 3 — Where does my program keep things?

### Build

`**process-lab**`

The program should contain:

- one global variable
    
- one local variable
    
- one value allocated using `malloc()`
    

Print the value and memory address of each.

Example output:

```
PID: 4021

global:
value = 10
address = 0x...

local:
value = 20
address = 0x...

allocated:
value = 30
address = 0x...

Press Enter to exit.
```

The program stays alive long enough for us to inspect it.

We also create one deliberately broken memory example and investigate what went wrong.

### What we learn

We begin exploring:

- addresses
    
- pointers
    
- memory
    
- stack
    
- heap
    
- `malloc()`
    
- `free()`
    

We use a debugger and operating-system tools to inspect our own running process.

### Why are we building this?

In JavaScript, memory management is mostly hidden from us.

Here, we want to see that variables exist somewhere in memory and that our program is operating on actual addresses.

We are building the mental model that later helps us understand memory bugs, performance, garbage collectors and Rust's ownership system.

---

## Session 4 — How does my program ask the OS to do things?

### Extend `process-lab`

Add a small file-copying feature.

Run:

```
./process-lab input.txt output.txt
```

The program should:

1. open `input.txt`
    
2. read data from it
    
3. create/open `output.txt`
    
4. write the data there
    
5. close both files
    

Use:

- `open()`
    
- `read()`
    
- `write()`
    
- `close()`
    

We also inspect which files the running process has open.

### What we learn

We introduce:

- system calls
    
- files
    
- file descriptors
    
- stdin
    
- stdout
    
- stderr
    
- the boundary between our program and the operating system
    

The key idea is:

```
our program
     ↓
asks the operating system
     ↓
operating system performs the operation
```

### Why are we building this?

A program cannot simply access every part of the machine however it wants.

The operating system controls important resources.

System calls are one of the main ways a program asks the OS to do work on its behalf.

File descriptors become important later because Unix uses the same basic idea for files, terminals, pipes and sockets.

---

# Week 3 — Programs Working Together

```
program A
    │
    │ output
    ▼
   pipe
    │
    │ input
    ▼
program B
```

## Session 5 — How do two programs work together?

### Build

Start `**tinysh**`.

Before building the full shell, reproduce one simple pipeline ourselves.

For example:

```
printf "apple\nbanana\napple\n" | grep apple
```

Our C program should:

1. create a pipe
    
2. create the first child process
    
3. create the second child process
    
4. connect the first program's output to the pipe
    
5. connect the second program's input to the pipe
    
6. run both programs
    
7. wait for them to finish
    

We will encounter:

- `pipe()`
    
- `fork()`
    
- `exec()`
    
- `dup2()`
    
- `wait()`
    

### What we learn

Processes do not have to work alone.

Unix programs can be connected so that the output of one becomes the input of another.

We also see that file descriptors are the connection between ideas we learned in Week 2 and process communication.

### Why are we building this?

We already use pipelines in the terminal.

Now we want to understand the machinery underneath:

```
command A
    ↓
file descriptor
    ↓
pipe
    ↓
file descriptor
    ↓
command B
```

This prepares us to build our own shell.

---

## Session 6 — Can we explain the whole journey?

### Finish `tinysh`

Our shell should display a prompt:

```
tinysh>
```

It should accept commands such as:

```
tinysh> echo hello
hello
exit status: 0

tinysh> ls -la

tinysh> exit
```

The shell should:

1. display a prompt
    
2. read a command
    
3. separate the program name from its arguments
    
4. create a child process
    
5. run the requested program
    
6. wait for it to finish
    
7. report its exit status
    
8. show the prompt again
    

Add **one** composition feature:

- one pipe
    

or

- output redirection
    

One is enough for Phase 1.

### Why are we building this?

`tinysh` combines almost everything from Phase 1.

```
user types command
        ↓
shell reads it
        ↓
shell creates a process
        ↓
process runs an executable
        ↓
program uses memory
        ↓
program talks to the OS
        ↓
program may use files or pipes
        ↓
program exits
        ↓
shell receives the result
```

If we can build and explain this ourselves, then the original question:

> **I type** `**./program**`**. What happens next?**

has a much more concrete answer.

---

# How We Learn

For every experiment:

```
WRITE
  ↓
PREDICT
  ↓
RUN
  ↓
INSPECT
  ↓
EXPLAIN
```

## Write

We write the code ourselves.

AI coding agents do not implement the exercises.

We can research using:

- books
    
- documentation
    
- `man` pages
    
- lectures
    
- articles
    
- source code
    
- examples
    
- AI for explanation or review
    

But we should understand every line that ends up in our implementation.

## Predict

Before running an experiment, state what we think will happen.

For example:

> After calling `fork()`, how many processes will execute the next line?

## Run

Execute it and observe what actually happens.

## Inspect

When something surprises us, investigate.

Use:

- compiler warnings
    
- debuggers
    
- sanitizers
    
- process inspection
    
- system-call tracing
    
- binary inspection
    
- operating-system documentation
    

## Explain

Both of us should eventually be able to explain the concept independently.

The goal is mastery through explaining it to each other.

If only one person understands it, we are not finished.

---

# Phase 1 Output

At the end we should have:

```
phase-1/
├── week-1-hello-systems/
├── week-2-process-lab/
└── week-3-tinysh/
```

Three implementations.

One continuous learning journey.

---

# Exit Question

At the end of Phase 1, either of us should be able to answer:

> **I type** `**./program**`**. What happens next?**

and follow the journey through:

```
source
↓
compiler
↓
linker
↓
executable
↓
process
↓
memory
↓
operating system
↓
files / input / output
↓
other processes
↓
exit
```

We do not need kernel-level knowledge yet.

We need a clear programmer's mental model of how a program becomes a running process and interacts with a Unix-like operating system.

---

# Reference Shelf

These are references for the journey. We do not need to read all of them at once.

- [Computer Systems: A Programmer's Perspective / CMU 15-213](https://csapp.cs.cmu.edu/)
    
- [Operating Systems: Three Easy Pieces](https://pages.cs.wisc.edu/~remzi/OSTEP/)
    
- [MIT xv6 / 6.1810](https://pdos.csail.mit.edu/6.S081/)
    
- [Beej's Guide to C](https://beej.us/guide/bgc/)
    
- [Beej's Guide to Network Programming](https://beej.us/guide/bgnet/)
    
- [Crafting Interpreters](https://craftinginterpreters.com/)
    
- [Computer, Enhance!](https://www.computerenhance.com/p/table-of-contents)
    
- [Web Browser Engineering](https://browser.engineering/)