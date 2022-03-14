# ICS-53

## Module 5 Part 2 Virtual Memory

- modern systems provide an abstraction of main memory known as ***virtual memory*** (VM)
  - 3 important capabilities
        1. uses main memory efficiently by treating it as a cache for an address space stored on disk
        2. simplifies memory management by providing each process with a uniform address space
        3. protects the address space of each process from corruption by other processes.

### 9.1 Physical and Virtual Addressing
- **PA physical address**
  - The main memory of a computer system is organized as an array of  ***M*** contiguous byte-sized cells. 
  - Each byte has a unique physical address (PA).
- With virtual addressing, the CPU accesses main memory by generating a **virtual address (VA)**
- The task of converting a virtual address to a physical one is known as **address translation**
- **memory management unit (MMU)** on CPU translates virtual addresses

### 9.2 Address Spaces
- An address space is an ordered set of nonnegative integer addresses
  - If the integers in the address space are consecutive, then we say that it is a ***linear address space***.
- the CPU generates virtual addresses from an address space of $N = 2^n$ addresses called the virtual address space: $\{0,1,2,...,N-1\}$
  - 32-bit $N=2^{32}$
  - 64-bit $N=2^{64}$
- A system also has a **physical address space** that corresponds to the $M=2^m$ bytes of physical memory in the system: {0,1,2,...,M-1}

### VM as a tool for Caching
- Conceptually, a virtual memory is organized as an array of N contiguous byte-sized cells stored on disk.
- VM systems handle this by partitioning the virtual memory into fixed-sized blocks called **virtual pages (VPs)**
  - Each virtual page is $P = 2^p$ bytes in size
  -  Similarly, **physical memory** is partitioned into **physical page (PPs)**, also P bytes in size
     - also referred to as ***page frames***.
- the set of ***virtual pages*** is partitioned into 3 disjoint
subsets
    - **Unallocated** Unallocated blocks ***do not have any data*** associated with them, and thus do not occupy any space on disk
    - **Cached** **Allocated** pages that are currently **cached** in physical memory
    - **Uncached** **Allocated** pages that are **not cached** in physical memory

#### 9.3.1 DRAM Cache Organization
- ***SRAM cache*** to denote the L1, L2, and L3 cache memories between the
CPU and main memory
- ***DRAM cache*** to denote the VM system’s cache that caches virtual pages in main memory
  - DRAM caches are fully associative

#### 9.3.2 Page Tables
- Page Tables
  - a data structure stored in **physical memory** known as a page table that maps virtual pages to physical pages.
  - 1 **0-1 valid bit** and an n-bit **address field**
    - whether the virtual page is currently cached in DRAM
- A page table is an array of **page table entries (PTEs)**.

#### 9.3.3 Page Hits
- it uses the **physical memory address in the PTE** (which points to the start of the cached page in PP 1) to construct the **physical address** of the word

#### 9.3.4 Page Faults
- a DRAM cache miss is known as a ***page fault***.
- The page fault exception invokes a ***page fault exception handler*** in the kernel
  - When the handler returns, it **restarts the faulting instruction**
- The activity of transferring a page between disk and memory is known as ***swapping or paging***
  - **swapped in (paged in)** from disk to DRAM
  - **swapped out (paged out)** from DRAM to disk
- The strategy of waiting until the last moment to swap in a page, when a miss occurs, is known as ***demand paging***

#### 9.3.5 Allocating Pages

#### 9.3.6 Locality to the Rescue Again
- As long as our programs have good **temporal locality**, virtual memory systems work quite well.

### 9.4 VM as a Tool for Memory Management
- Simplifying linking
  - A separate address space allows each process to use the **same basic format** for its ***memory image***
- Simplifying loading
  - easy to load executable and shared object files into memory
    - .data .text
- Simplifying sharing
  - sharing between user processes and the operating system itself.
- Simplifying memory allocation

### 9.5 VM as a Tool for Memory Protection
- Segmentation Fault
  - If an instruction violates these permissions, then the CPU triggers a general protection fault that transfers control to an exception handler in the kernel

### 9.6 Address Translation
- address translation is a mapping between the elements of an **N-element virtual address space (VAS)** and an **M-element physical address space(PAS)** $$MAP: VAS \rightarrow PAS ∪ \empty$$