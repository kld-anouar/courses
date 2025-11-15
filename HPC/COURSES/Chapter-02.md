### **Chapter II: Unlocking Parallel Power: Programming Models and Tools**

**Course:** High-Performance Computing / Introduction to Parallel Computing  
**Instructor:** Dr. Anouar Khaldi

---

### **1. How to Think in Parallel: Core Strategies**

Before writing a single line of parallel code, we need a game plan. We need to break down a **big problem** into **smaller pieces** that computers can solve at the same time? In this section, we’ll explore three fundamental paradigms that shape how we design parallel algorithms:

1. **Task Parallelism** — different tasks run in parallel
2. **Data Parallelism** — same task applied to different data
3. **Hybrid Parallelism** — a combination of both

#### 1.1 Task Parallelism Paradigm 

- Task parallelism is dividing a program into independent tasks.
- Each task performs a different operation.
- These tasks can run at the same time on separate processors or cores.

**Visual Model:**
```
┌─────────────┐
│ Task 1      │
│ (Read Data) │
└──────┬──────┘
       │
       ├─────────────────────┬──────────────────────┐
       │                     │                      │
┌──────▼────────┐   ┌────────▼──────────┐   ┌───────▼───────┐
│ Task 2        │   │ Task 3            │   │ Task 4        │
│ (Process A)   │   │ (Process B)       │   │ (Process C)   │
│ [In Parallel] │   │ [In Parallel]     │   │ [In Parallel] │
└──────┬────────┘   └────────┬──────────┘   └───────┬───────┘
       │                     │                      │
       └───────────┬─────────┴──────────┬───────────┘
            ┌──────▼────────┐   ┌───────▼───────┐
            │ Task 5        │   │ Task 6        │
            │ (Process D)   │   │ (Process E)   │
            │ [In Parallel] │   │ [In Parallel] │
            └──────┬────────┘   └───────┬───────┘
                   └─────────┬──────────┘
                             │
                     ┌───────▼────────┐
                     │     Task 7     │
                     │ (Write Result) │
                     └────────────────┘
```

**Key Characteristics**
- **Independent execution**: Tasks run without interfering with each other.
- **Synchronization points**: Some tasks must finish before others can begin.
- **Load balancing challenge**: If one task takes much longer than others, some processors may remain idle.

**When is it useful?** This approach is great for building "pipelines", where data flows through a series of **processing stages**, like in video editing or complex data analysis.



#### 1.2 Data Parallelism Paradigm

- Data parallelism is dividing the data into chunks.
- Each chunk is processed independently.
- The same operation is performed on each chunk of data simultaneously on separate processors or cores.

**Visual Model**
```
 Original Data [1, 2, 3, 4, 5, 6, 7, 8]
         │
         ├─ Split into 4 chunks for 4 processors
         │
Chunk 1: [1, 2]    → Processor 1
Chunk 2: [3, 4]    → Processor 2
Chunk 3: [5, 6]    → Processor 3
Chunk 4: [7, 8]    → Processor 4
         │
         ├─ All processors do the same thing: multiply by 2
         │
Result 1: [2, 4]
Result 2: [6, 8]
Result 3: [10, 12]
Result 4: [14, 16]
         │
         └─ Combine results: [2, 4, 6, 8, 10, 12, 14, 16]
```

**Key Characteristics**
- **Data independence**: Each processor works on its own portion of the data without interfering with others.
- **Synchronized processing**: Processors perform the same operation at the same time.
- **Scalability**: Performance can be increased by simply adding more processors to handle more data chunks.
- **Effective load balancing**: Since each processor performs the same task on similar-sized data chunks, the workload is evenly distributed.


**When is it useful?** This is the go-to strategy for scientific computing, image processing, and anything involving large arrays or matrices.



#### **1.3 Hybrid Parallelism: The Best of Both Worlds**

Why choose one when you can use both? Hybrid Parallelism combines Task and Data Parallelism.

- Hybrid parallelism combines both task and data parallelism.
- A program is first broken down into a set of independent tasks (Task Parallelism).
- Within each task, the data is partitioned and the same operation is applied to each partition across multiple processors (Data Parallelism).

**Visual Model**
```
        Task Parallelism (different tasks running at same time)
┌──────────────────────┬──────────────────────┬──────────────────────┐
│        Task 1        │        Task 2        │        Task 3        │
│                      │                      │                      │
├── Cores 1–4          ├── Cores 5–8          ├── Cores 9–12         │
│   (Data Parallelism) │   (Data Parallelism) │   (Data Parallelism) │
│   [Data Chunks]      │   [Data Chunks]      │   [Data Chunks]      │
│   ↓                  │   ↓                  │   ↓                  │
└──────────────────────┴──────────┬───────────┴──────────────────────┘
                                  │
                           ┌──────▼──────┐
                           │   Combine   │
                           │   Results   │
                           └─────────────┘
```

**Key Characteristics**
- **Hierarchical structure**: Organizes parallelism on two levels, fitting complex problems.
- **Architectural adaptability**: Maps well to modern computing clusters where each node has multiple cores.
- **Reduced communication**: Limits intensive communication to small groups of processors working on the same sub-task.
- **Increased complexity**: Requires more planning and synchronization.

**When is it useful?** This approach is essential for large-scale applications running on supercomputers or distributed clusters, such as complex scientific simulations, climate modeling, and training very large neural networks.


### **2. Communicating Between Computers: The Message Passing Interface (MPI)**

- MPI is a standardized communication protocol for parallel computing in distributed memory systems.
- It enables processes running on different nodes (computers) to send and receive messages, allowing them to coordinate and share data.
- Each process operates within its own private memory space and communicates explicitly through the network.

#### **2.1 Core MPI Concepts**

*   **Communicator:** The group of processes that are working together. `MPI_COMM_WORLD` is the default group that includes every process you started.
*   **Rank:** Unique ID for each process (0 to N-1) within a communicator.
*   **Message:** Data sent between processes, including sender rank, receiver rank, and a tag.

**Process Model: Independent Processes, Connected by a Network**
```
┌─────────────┐     ┌─────────────┐     ┌─────────────┐
│  Process 0  │     │  Process 1  │     │  Process 2  │
├─────────────┤     ├─────────────┤     ├─────────────┤ 
│  Own Memory │     │  Own Memory │     │  Own Memory │
└─────┬───────┘     └─────┬───────┘     └─────┬───────┘
      │                   │                   │
      └───────────────────┼───────────────────┘
                 Communication Network
                 (MPI_Send / MPI_Recv)
```

#### **2.2 Basic MPI Functions**

**Point-to-Point Communication: A Direct Conversation**


This is like making a phone call from one process to another.

*   `MPI_Send`: One process packages data and sends it to a specific destination rank.
*   `MPI_Recv`: Another process waits to receive a package from a specific source rank.

**Example: Basic Send and Receive in C**

```c
#include <mpi.h>
#include <stdio.h>

int main(int argc, char** argv) {
    MPI_Init(&argc, &argv);  // Start MPI environment

    int rank, size;
    MPI_Comm_rank(MPI_COMM_WORLD, &rank); // get process rank
    MPI_Comm_size(MPI_COMM_WORLD, &size); // get processes count

    if (size < 2) {
        printf("Run with at least 2 processes!\n");
        MPI_Finalize(); // exit
        return 0;
    }

    if (rank == 0) {
        int data = 42;
        MPI_Send(&data, 1, MPI_INT, 1, 0, MPI_COMM_WORLD);
        printf("Process 0 sent data: %d\n", data);
    } else if (rank == 1) {
        int received;
        MPI_Recv(&received, 1, MPI_INT, 0, 0, MPI_COMM_WORLD, MPI_STATUS_IGNORE);
        printf("Process 1 received data: %d\n", received);
    }

    MPI_Finalize();  // End MPI environment
    return 0;
}
```

```c
mpicc mpi_sendrecv.c -o mpi_sendrecv // Compile
mpirun --oversubscribe -np 2 ./mpi_sendrecv // Run 2 processes
```

**Output:**

```
Process 0 sent data: 42
Process 1 received data: 42
```
This simple program shows two processes exchanging data directly.


**Example: Parallel Sum of an Array (Data Parallelism)**

Data parallelism using `MPI_Scatter` and `MPI_Reduce`.

```c
#include <mpi.h>
#include <stdio.h>
#include <stdlib.h>

int main(int argc, char** argv) {
    MPI_Init(&argc, &argv);

    int rank, size;
    MPI_Comm_rank(MPI_COMM_WORLD, &rank);
    MPI_Comm_size(MPI_COMM_WORLD, &size);

    int N = 16;
    int *data = NULL;
    int chunk = N / size;
    int *local = (int*)malloc(chunk * sizeof(int));

    if (rank == 0) {
        data = (int*)malloc(N * sizeof(int));
        for (int i = 0; i < N; i++) data[i] = i + 1;  // Fill with 1..N
    }

    MPI_Scatter(data, chunk, MPI_INT, local, chunk, MPI_INT, 0, MPI_COMM_WORLD);

    int local_sum = 0;
    for (int i = 0; i < chunk; i++) local_sum += local[i];

    int global_sum = 0;
    MPI_Reduce(&local_sum, &global_sum, 1, MPI_INT, MPI_SUM, 0, MPI_COMM_WORLD);

    if (rank == 0) {
        printf("Global sum = %d\n", global_sum);
    }

    free(local);
    if (rank == 0) free(data);
    MPI_Finalize();
    return 0;
}
```

**Expected Output:**

```
Global sum = 136
```

**Blocking Explained:** A standard `MPI_Send` will *block*—meaning the process will pause and wait—until the message is safely on its way. `MPI_Recv` will block until the message it's waiting for arrives. This is crucial for making sure data is sent and received correctly.

**Collective Communication: A Team Meeting**

These functions involve every process in the communicator participating at once.

*   `MPI_Bcast` **(Broadcast):** The "memo." One process has a piece of data, and it sends a copy to everyone else.


**Example: Broadcasting a Value**

```c
#include <mpi.h>
#include <stdio.h>

int main(int argc, char** argv) {
    MPI_Init(&argc, &argv);

    int rank;
    MPI_Comm_rank(MPI_COMM_WORLD, &rank);

    int value;
    if (rank == 0) {
        value = 100;
        printf("Process 0 broadcasting value %d\n", value);
    }

    MPI_Bcast(&value, 1, MPI_INT, 0, MPI_COMM_WORLD);
    printf("Process %d received broadcast value %d\n", rank, value);

    MPI_Finalize();
    return 0;
}
```

**Output (4 processes):**

```
Process 0 broadcasting value 100
Process 1 received broadcast value 100
Process 2 received broadcast value 100
Process 3 received broadcast value 100
```

This demonstrates how MPI synchronizes data across all processes.


#### **2.3 Common Pitfalls: Deadlock**

A major challenge in message passing is **deadlock**. This happens when two or more processes are stuck waiting for each other in a circular chain.

**Example of Deadlock:**

```c
// Both processes try to send before receiving – will hang!
if (rank == 0) {
    MPI_Send(&data, 1, MPI_INT, 1, 0, MPI_COMM_WORLD);
    MPI_Recv(&data, 1, MPI_INT, 1, 0, MPI_COMM_WORLD, MPI_STATUS_IGNORE);
} else if (rank == 1) {
    MPI_Send(&data, 1, MPI_INT, 0, 0, MPI_COMM_WORLD);
    MPI_Recv(&data, 1, MPI_INT, 0, 0, MPI_COMM_WORLD, MPI_STATUS_IGNORE);
}
```

Both processes call `MPI_Send` and wait for the other to receive. Neither reaches the `MPI_Recv`, so both block forever.

**Advantages of MPI:**
*   Can scale to thousands of computers.
*   Gives you precise control over communication.
*   Works on virtually any multi-computer system.

**Disadvantages of MPI:**
*   Requires you to manually manage all communication, which can be complex.
*   Deadlocks and other bugs are easy to create and hard to find.



### **3. Cooperating on One Computer: OpenMP**

- Multi-core processors share the same memory within a single computer (shared memory system).
- OpenMP helps easily parallelize code in such environments.
- It uses pragmas (special comments) added to the code.
- These pragmas tell the compiler which code sections should run in parallel.

#### **3.1 Core OpenMP Concepts**

-   **Thread:** A single path of execution. A multi-core CPU can run multiple threads at once.
-   **Fork-Join Model:**  The program starts as one main thread, forks into worker threads for parallel sections, then joins back to continue.

**Visual Model: The Fork-Join Lifecycle**
```
      Main Thread (Sequential)
               │
     #pragma omp parallel (FORK)
               │
   ┌────┬────┬─┴──┬────┬────┬
   │    │    │    │    │    │
   │ T0 │ T1 │ T2 │ T3 │ T4 │.. ← Team of threads in parallel
   │    │    │    │    │    │
   └────┴────┴─┬──┴────┴────┴
               │
             (JOIN)
               │
        Main Thread (Sequential again)
```

**Example: Simple OpenMP**
```c
#include <omp.h>
#include <stdio.h>

int main() {
  #pragma omp parallel
  {
    int tid = omp_get_thread_num();
    int n   = omp_get_num_threads();
    printf("Hello from thread %d of %d\n", tid, n);
  }
  return 0;
}
```

**Expected Output (order may vary):**
```
Hello from thread 0 of 4
Hello from thread 1 of 4
Hello from thread 2 of 4
Hello from thread 3 of 4
```

#### 3.2 Worksharing: Parallel Loops & Blocks

**Parallel Loops**

The most common use of OpenMP is to split up the work of a `for` loop.

```c     
#pragma omp parallel for
for (int i = 0; i < 1000; i++) {
    // Each thread will automatically get a different set of 'i' values
    // eg. 3 threads: t1 -> [0-333], t2 -> [334-666], t3 -> [667-999]
    // and run this code at the same time.
}
```
* **Scheduling** controls *which* iterations go to *which* thread:

  * `schedule(static[,chunk])` – pre-chunked, low overhead (best for uniform work).
  * `schedule(dynamic[,chunk])` – threads pull work as they finish (better for uneven work).
  * `schedule(guided[,chunk])` – decreasing chunk sizes over time (load balancing with less overhead).

**Example: SAXPY (y = a*x + y)**

```c
#pragma omp parallel for schedule(static)
for (int i = 0; i < n; ++i)
  y[i] = a * x[i] + y[i];
```

#### 3.3 Different independent blocks in parallel
Use `sections` when you have separate blocks of work that can run at the same time — not loop iterations, but different functions or tasks.

```c
#pragma omp parallel sections
{
  #pragma omp section
  { render_video(); }        // one task (e.g., read/write data)

  #pragma omp section
  { play_music(); }   // another independent task (e.g., calculations)
}
```
How it works:
- The parallel sections block creates a team of threads.
- Each section is assigned to a different thread (if available).
- All sections run concurrently.
- When all sections finish, threads synchronize before continuing.

#### 3.3 `single` & `master`: one thread executes a block
Sometimes, we want **only one thread** to execute a certain part of the code —  for example, printing, loading data once, or doing setup work.

- **`single`** – The block is executed by **only one arbitrary thread**, while others skip it.  
  - Has an **implicit barrier** at the end (threads wait), unless you add `nowait` to skip that waiting.

- **`master`** – The block is executed **only by thread 0**.  
  - **No implicit barrier**, so other threads continue immediately.


**Example:**

```c
#include <omp.h>
#include <stdio.h>

int main() {
  #pragma omp parallel
  {
    int tid = omp_get_thread_num();

    // executed by one arbitrary thread
    #pragma omp single
    {
      printf("Thread %d: loading data (single)\n", tid);
    }

    // code executed by all threads 
    printf("Thread %d: working...\n", tid);

    // executed only by master thread (thread 0)
    #pragma omp master
    {
      printf("Thread %d: saving results (master)\n", tid);
    }
  }
  return 0;
}
```

#### 3.4 Data Scoping & Reductions

When OpenMP runs code in parallel, it must know which variables are **shared** between threads and which are **private** to each one.

* **`shared(var)`** – All threads see and can modify the *same* variable in memory. This is the default behavior for global or static variables.

  * Example: Shared counters, arrays, or data structures accessed by all threads.

* **`private(var)`** – Each thread gets its *own independent copy* of the variable. Changes made by one thread are *not visible* to others.

  * Example: Loop indices or temporary variables that each thread should handle separately.

* **`reduction(+:sum)`** – Used when you need to *safely combine results* from all threads using an operation like `+`, `*`, `min`, or `max`.

  * Each thread keeps a private version of the variable (e.g., `sum`), and OpenMP automatically merges all results at the end using the specified operator.


**Example: Factorial of N**
```c
#include <omp.h>
#include <stdio.h>

int main() {
  const int N = 6;       // compute factorial of 10
  long long factorial = 1;

  #pragma omp parallel for default(none) shared(N) reduction(*:factorial)
  for (int i = 1; i <= N; ++i)
    factorial *= i;

  printf("%d! = %lld\n", N, factorial);
  return 0;
}
```

Each thread calculates its own range of values, and then all partial results are combined using the multiplication (*) operation into the final result of the sequential program.


### 3.5 Synchronization: Avoiding Races & Ordering Issues

When multiple threads work on shared data, they can **interfere** with each other — this is called a **race condition**.  
OpenMP provides several tools to synchronize threads and ensure correct, predictable results.


#### 🔹 Common Synchronization Constructs

| Directive | Purpose | Example Use |
|------------|----------|-------------|
| `#pragma omp critical` | Only one thread at a time executes the block (a general lock). | Updating a shared counter or array. |
| `#pragma omp atomic` | A lightweight version for a single variable update (e.g. `x++`, `sum += v`). | Safe increments without full locking. |
| `#pragma omp barrier` | All threads must reach this point before continuing. | Wait for all threads to finish a phase. |
| `#pragma omp ordered` | Forces iterations to follow the original order. | Print results in loop order. |


### Example: Safe Counter Update

```c
#include <omp.h>
#include <stdio.h>

int main() {
  const int N = 1000000;
  long long count = 0;

  // Multiple threads increment a shared counter
  #pragma omp parallel for
  for (int i = 0; i < N; ++i)
    count++;   // race condition here

  printf("Incorrect count = %lld\n", count);

  count = 0; // reset

  // FIX: atomic ensures only one thread updates at a time
  #pragma omp parallel for
  for (int i = 0; i < N; ++i)
    #pragma omp atomic
    count++;

  printf("Correct count = %lld\n", count);
  return 0;
}
```

**Advantages of OpenMP:**
*   Very easy to start with; you can parallelize existing code incrementally.
*   Requires minimal code changes.
*   Excellent performance for multi-core CPUs.

**Disadvantages of OpenMP:**
*   Only works on a single, shared-memory machine.
*   Cannot be used for clusters of computers.
*   Can have performance issues if threads are constantly waiting for each other.

---

### **4. The Expert's Toolkit: PThreads**

PThreads (POSIX Threads) is a low-level, manual approach to threading. It gives you maximum control but also maximum responsibility. Unlike OpenMP's simple pragmas, with PThreads, you manually create, manage, and synchronize every thread.

#### **4.1 Key PThreads Tools**

*   `pthread_create()`: Manually starts a new thread.
*   `pthread_join()`: Makes the main program wait for a thread to finish.

**Synchronization Primitives:**

*   **Mutex (Mutual Exclusion):** A mutex is a lock. Before a thread can access a shared variable, it must `lock` the mutex. When it's finished, it must `unlock` it. Any other thread that tries to lock it while it's already locked will be forced to wait. This is the primary tool to prevent race conditions.
*   **Condition Variables:** These allow threads to wait efficiently for a specific condition to become true. A thread can lock a mutex, check a condition, and if it's not ready, it can `wait` on a condition variable. This automatically unlocks the mutex and puts the thread to sleep. When another thread makes the condition true, it can `signal` the condition variable to wake up the waiting thread.

**Advantages of PThreads:**
*   Provides complete, fine-grained control over every aspect of threading.
*   The standard for creating complex, multi-threaded applications like web servers or databases.

**Disadvantages of PThreads:**
*   Much more complex and verbose than OpenMP.
*   Extremely easy to make mistakes that lead to bugs like deadlocks and race conditions.
*   Difficult to debug.

---

### **5. Summary: Choosing the Right Tool**

| Aspect | MPI | OpenMP | PThreads |
| :--- | :--- | :--- | :--- |
| **Best For** | Multiple Computers (Clusters) | Single Multi-Core Computer | Single Multi-Core Computer |
| **Scalability** | Extremely High | Medium (limited by cores) | Medium (limited by cores) |
| **Ease of Use** | Difficult | Easy | Difficult |
| **Communication** | Explicit Messages | Shared Variables | Shared Variables |
| **Synchronization** | Done through messages | Simple pragmas | Manual locks & conditions |
| **Code Changes** | High | Low | High |

**A Simple Guide to Choosing:**
1.  Are you running on a **cluster of computers**?
    *   **Yes:** You must use **MPI**.
2.  Are you running on a **single multi-core computer**?
    *   Do you want to parallelize quickly and easily, especially loops? Use **OpenMP**.
    *   Do you need complete control over complex thread interactions? Use **PThreads**.

---

### **6. The Modern Approach: Hybrid Programming (MPI + OpenMP)**

In modern supercomputing, the most common and powerful approach is to combine MPI and OpenMP.

*   **MPI is used *between* computers (nodes).** It handles the coarse-grained communication across the network.
*   **OpenMP is used *inside* each computer.** It handles the fine-grained parallelism across the multiple cores of a single node.

This hybrid model reduces the number of MPI messages that need to fly across the slow network, leading to better performance and scalability.

**Example on a 4-node cluster (each with 8 cores):**
You would run 4 MPI processes (one per node). Each of those 4 processes would then create 8 OpenMP threads to take full advantage of the cores on its node.