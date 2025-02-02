---
layout: single
title:  "Book Club: High Performance Python (Chapter 1)"
date:   2025-02-02
category:
    book-club
tags:
  - python
  - learning
  - book-club
---

Today I took the first step in pushing for a more formal understanding of optimisation in Python. I often need things to run faster or with lower memory, but achieving this comes down to interactively checking things with `memory_profiler` and `%memit` in ipython, or the built-in `%timeit`, comparing different versions of functions and going from there. 

Now I know this is suboptimal. I know I need to do things more formally, but finding the time to push for learning this in your everyday job is hard. So here's my first step - chapter 1 of a book club going through the whole of [High Performance Python]() by Micha Gorelick and Ian Ozwald. I'll be going through the whole book and summarising the main points as I go. Mainly for me, but also as a mini-summary for others.

The main stand-out points for me were:

- memory capacity and read/write speeds are inversely related
- a lot of the improvements in these speeds are actually about the connections between them, including the distance of the connection
- the idea of **heavy data** - a way of thinking that emphasises that data takes time and effort to move around, and should be avoided where possible

## The bones of high performance computing

The crux of things, in terms of performance, comes down to compute, memory, and connections. That's to say:

- compute: CPUs or GPUs
- memory: RAM, cache, registers, and storage (hard drives)
- connections: internal (between memory and compute) or external (to a network)

Something I underappreciated was the importance of not just the capacity of memory but the different read/write speeds and how important they are.

## Compute in detail

For CPUs, we measure how good they are in two ways:

- Instructions Per Cycle (IPC) - the number of instructions a CPU can perform in a single clock cycle (note not all instructions use a single operation)
- Cycles per second (clock speed)

As they note in the book, these are often competing: IPC may go up but clock speed goes down. They don't cover why this is really. GPUs however can be high on both but are instead limited in connection speed.

Optimising them also achieves different things. Higher clock speed is good all round - more calculations per second - but higher IPC changes how much vectorisation is possible i.e. the CPU doing more than one thing at a time. To be clear, it's a single instruction, but multiple operations because it does the same thing on multiple bits of data. This is called single instruction multiple data (SIMD).

In relation to all of this, improvements in clock speed and IPC are important, but their figure 1-1 shows that they started hitting a wall in about 2005. Instead, manufacturers turned to hyperthreading (where a single physical CPU is represented virtually to the OS as two CPUs, and operations sent to the second vCPU are run on the same physical hardware but as separate logical threads while operations of the first vCPU are waiting for something, increasing but not doubling performance), out-of-order execution (where the CPU runs things in a more optimal order if there are linear and not dependent on the code before), or multi-core architecture (effectively multiple CPUs repackaged as a single CPU but still sharing a cache and memory bus, so that the 'core' is now the independent operational unit but cores within a single CPU communicate faster than do separate CPUs). This all basically amounts to more hardware/CPUs available (virtually through hyperthreading or physically through multi-core architectures) or more optimal code (through out-of-order execution), rather than improvements to the CPU itself. This is important for us in python, as we need to take advantage of multiple processes and hence often multiple cores because of the GIL (more on this later).

Note: hyperthreading is distinct from multithreading. The former is a CPU level optimisation that represents a physical CPU as multiple virtual CPUs to increase the amount of time CPUs spend actually performing operations. By contrast, multithreading is a lightweight form of parallel computing where the functions and memory are inherited from the parent process and multiple threads are run simultaneously. If that's on a single core, then 'simultaneously' actually means through time-slicing, where they quickly take turns using a single core. This is a software-level optimisation.

## Memory in detail

The core points for memory are:

- it's mostly faster when data are read sequentially (all from the same place in one chunk) rather than at random (spread out in smaller chunks)
- a lot of memory optimisation reduces to which type of memory we use (where data is located), making sure we get sequential reads, that we do transfers once (minimising how much data is moved around)
- on top of the speed of actual read/writes, there is a latency associated with each type of memory when it needs to find the data or space for it. This is the speed to respond to a request, and it's an ever-present overhead for operations. Naturally, it's faster on RAM and slower on a spinning disk (where a head needs to physically move and the disk needs to be spun up).

Knowing what those memory types are is important. I think everyone is familiar with spinning disks, solid state drives and RAM, and we know that capacity in these decreases as read/write speeds increase. Less well known are cache (L1 and L2, maybe L3 and L4) and registers. The important things here are that:

- L1/L2 cache have extremely fast read/write operations, very small capacity (kilobytes)
- all data heading to/from the CPU _**must**_ go through at least L1 cache (apparently some bypass L2)
- Hard disks in particular struggle with random access patterns, but RAM and others are relatively good at this
- Techniques like asynchronous I/O and pre-emptive caching are used to make sure data is where it needs to be at the right time, so there aren't delays

## Connections in detail

All of these are apparently variations on a simple bus connection. Big ones are:

- the frontside bus - connects RAM to L1/L2
- the backside bus - connects L1/L2 to the CPU
- the external bus - connects hardware (hard drives, the NIC) to the CPU

The backside bus is the fastest, and hence the more we can keep data in L1/L2 so any repeated transfer to the CPU happen via the backside bus only, the better. The frontside bus is slower. Hence, we don't want to repeatedly move data from RAM to L1/L2 where possible, to avoid the speed costs of the transfer. A lot of the speed connections from cache over RAM appear to actuall be due to the faster speed of the backside bus!

GPUs, which I mentioned can have high iterations per cyclem, or IPC (though GPUs run many simple operations in parallel), and clock speed, are usually an external bit of kit and so connect via the PCI bus, making there connection the limiting factor in speed by a long way. This is why some machines have an integrated GPU alongside the CPU.

Note that when I say the external connection is slower, I mean a lot slower. We're talking tens of GB/s for the frontside bus alone, but only Mb/s for external network connections like ethernet.

Busses get a bit more attention in this section, noting that they two ways they are measured:

- bus width - amount per transfer. Is actually how many physical wires, so it really is width
- bus frequency - number of transfers per second

Data moved in a single transfer must be sequential, meaning that it's always better to have sequential data here as it will get moved in one transfer if the width allows (hence high width is good for a lot of sequential data). Conversely we might want to increase bus frequency if we expect a lot of random data. 

## Optimising things in detail

I think most of the material here is covered elsewhere and reiterated in some form. It's worth noting though their example they give for vectorisation doesn't work (as they briefly note later, but it could use more prominence!). This is actually an example of chunking, where they move through a loop in chunks. This is because they're using python lists, which are not held contiguously in memory, but are rather a series of pointers to random data. As each of those chunks are arbitrary in size I imagine optimising anything to do with python lists would be an absolute pain or impossible.

The other thing to note is what vectorisation and SIMD really mean. I know of vectorisation in `R` and `numpy`, where it's referred to at a higher level to mean an operation performed on an array that's actually written in C under the hood and heavily optimised. What this means at a lower level is that CPU-level vectorisation can be achieved, whereby multiple values are sent to slots in vector registers (the fastest, smallest level of memory mentioned here) and one operation is done multiple times together.

Lastly, they note all the places where python falls short and makes our lives difficult (we all know why it's great, because that's why we're still here!). This amounts to:

- the automatic garbage collection, leading to fragmented memory and hence less chance of sequential reads
- python is dynamically typed, meaning types may change at runtime and we can't take advantage of certain optimisations
- python isn't compiled, and so can't do fun things like in C where you can optimise layout in compilation (we also don't have to deal with accidentally accessing data beyond the final index of an array and being return an actual value that could be confused for genuine data and crying when we find the error later)

## What's changed?

I originally read the 1st edition of this booked, which 
is a few years old now. Looking at the pre-release version of the 3rd edition, it seems not much has changed for this chapter. There's now a wide range of somewhat tangentially-related advice on programming added. Most of this is just general good programming advice that I won't repeat here. The York Notes are:

- to be an effective high performance programmer, don't optimise first. Instead, make it work (a rough script that works), then make it right (tests, docs etc.), then make it fast (optimise!)
- the GIL removal [that has been proposed in PEP 703](https://peps.python.org/pep-0703/) may be here aorund 2028
- a JIT compiler built into python may be here soon! See [PEP 744](https://peps.python.org/pep-0744/), for example.

## Summary

Phew! That was a lot for a quick tour. But now I (and you) know the basics. I think most was known to me, and I was aware of many of the things like L1/L2 cache, but the explicit features which explain why things are faster or slower weren't clear to me before. On to chapter 2!