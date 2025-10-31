## Chapter II: Models and Application Structures for Parallel Programming Interfaces (Expanded Edition)

In this chapter, we explore the **main models and interfaces** used to write parallel programs. By understanding these models deeply, we will later apply them in Practice Sessions (TD) and Labs (TP).

We continue the style of Chapter 1: **clear explanations, examples, diagrams, and comparisons**.



## 1. Paradigms for Parallel Applications

Parallel paradigms describe how we split work across computing resources.

### 1.1 Task Parallelism

Different **tasks/functions** run simultaneously.

**Real-world analogy:** In a restaurant kitchen

* Chef grills meat
* Assistant cuts vegetables
* Waiter serves food

All tasks are different but contribute to one goal.

**Code-style Example (conceptual):**

```
task wash_dishes
Task cook
Task set_table
```

All done in parallel.

**Used when:** tasks are independent and real work differs.



### 1.2 Data Parallelism

Same task, different data chunks.

**Example: Vector scaling**

```
A = [1 2 3 4]
Each core multiplies part of A by 2
```

Graphically:

```
Data: [A1 | A2 | A3 | A4]
Core1 → A1
Core2 → A2
Core3 → A3
Core4 → A4
```

**Used when:** working on large datasets (images, matrices, arrays).



### 1.3 Hybrid Parallelism (Task + Data)

Used in high-performance computing and AI.

**Example:** CPU schedules tasks, GPU handles matrix operations.

**Why hybrid?** Modern systems combine technologies:

* CPU = control & coordination
* GPU = massive data parallelism

This is how deep learning works.


### Summary Table

| Model         | Example System          | Best For                   |
| ------------- | ----------------------- | -------------------------- |
| Task Parallel | Web servers, OS threads | Different operations       |
| Data Parallel | GPUs, SIMD, HPC         | Same operation on big data |
| Hybrid        | CPUs + GPUs             | AI, scientific computing   |


## 2. Message Passing Interface (MPI)

MPI is the dominant model for **distributed memory systems** (clusters).

### 2.1 Concept

Each process has its **own memory** → communication required.

```
Process 1  ── send →  Process 2
Process 2  ── recv ←  Process 1
```

### 2.2 Key MPI Operations

| Function        | Meaning                   |
| --------------- | ------------------------- |
| `MPI_Init`      | Start MPI                 |
| `MPI_Finalize`  | End MPI                   |
| `MPI_Comm_rank` | Get process ID            |
| `MPI_Comm_size` | Get number of processes   |
| `MPI_Send`      | Send data                 |
| `MPI_Recv`      | Receive data              |
| `MPI_Bcast`     | Broadcast from one to all |

---

### 2.3 Mini Example (High Level)

```
Rank 0 asks: "What number do you have?"
Rank 1 replies: "I have B"
Rank 2 replies: "I have C"
```

Then Rank 0 collects answers.

We will later implement this in C MPI.

**Use cases:** Weather simulation, scientific HPC, cluster computing.

---

## 3. Parallel Programming with OpenMP

OpenMP is used in **shared memory (SMP, NUMA)**.

### 3.1 Parallelizing Loops

Add a pragma to split loop into threads.

Example (concept):

```
#pragma omp parallel for
for i in range(0..1000):
    A[i] = B[i] * 2
```

Threads share memory → fast communication, but race conditions may occur.

### 3.2 Synchronization Tools

| Feature     | Purpose                     |
| ----------- | --------------------------- |
| `critical`  | Only one thread executes    |
| `barrier`   | All threads must meet       |
| `private`   | Thread has its own variable |
| `reduction` | Safe parallel accumulation  |

---

## 4. PThreads (POSIX Threads)

Low‑level threading API → **maximum control**.

We define:

* thread functions
* thread creation
* mutex locks
* joins

Example idea:

```
create Thread1 → compute A
create Thread2 → compute B
lock mutex → update shared sum
join threads
```

**Why use PThreads?**

* Real‑time systems
* OS‑level scheduling
* When programming control matters more than simplicity

---

## Key Differences

| Feature  | MPI             | OpenMP            | PThreads            |
| -------- | --------------- | ----------------- | ------------------- |
| Memory   | Distributed     | Shared            | Shared              |
| Ease     | Hard            | Easy              | Medium‑Hard         |
| Scale    | Very high       | System‑local      | System‑local        |
| Control  | Message control | Directive‑based   | Full thread control |
| Used For | Clusters, cloud | Multicore servers | OS‑level threading  |

---

## Coming Next: Practical Work (TD & TP)

We will later perform:

* MPI programs: sending, broadcasting, summing numbers
* OpenMP loops & reductions
* PThreads thread creation + mutex exercises

We will also add:

* Real C code templates
* Performance measurement exercises
* Debugging common errors
* Mini‑projects (matrix multiply, parallel sort)



## Key Takeaways

Parallel interfaces differ by **memory model**, **control level**, and **difficulty**.
We master them step‑by‑step:

1. Understand theory
2. Practice structured examples
3. Solve real problems
