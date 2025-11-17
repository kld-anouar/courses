## **Chapter 2: Communicating Between Computers with MPI (Message Passing Interface)**

In distributed parallel computing, each process has its own private memory and executes independently. To collaborate, these processes must exchange data through a communication protocol. The **Message Passing Interface (MPI)** is the most widely used standard for this purpose.

MPI enables multiple processes, often running on different machines within a network, to send and receive messages. This allows them to coordinate computations, share intermediate results, and work together efficiently to solve large-scale problems.

---

### **2.1 Core MPI Concepts**

Before diving into the code, let’s understand the core ideas behind MPI:

* **Communicator:** A group of processes that can communicate with each other. `MPI_COMM_WORLD` is the default communicator that includes all processes launched by the program.
* **Rank:** Each process in a communicator has a unique ID (rank), ranging from 0 to N-1. The rank determines the role of each process.
* **Message:** The data transmitted between processes, which includes the content, the sender’s rank, the receiver’s rank, and a tag to identify the message type.

**Illustration: Independent Processes Connected by a Network**

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

Each process has its own memory but communicates explicitly via MPI calls through a network.

---

### **2.2 Basic MPI Functions**

MPI provides two main forms of communication: **point-to-point** and **collective**.

#### **Point-to-Point Communication: Direct Messaging**

This type of communication involves data transfer between exactly two processes.

* **`MPI_Send`** – Sends a message to another process.
* **`MPI_Recv`** – Receives a message from another process.

**Blocking Communication:**

A standard `MPI_Send` or `MPI_Recv` is *blocking*, meaning the process waits until the operation completes. For `MPI_Send`, this ensures the message is safely sent; for `MPI_Recv`, it ensures the message has arrived.

**Example: Basic Send and Receive in C**

```c
#include <mpi.h>
#include <stdio.h>

int main(int argc, char** argv) {
    MPI_Init(&argc, &argv);  // Start MPI environment

    int rank, size;
    MPI_Comm_rank(MPI_COMM_WORLD, &rank);
    MPI_Comm_size(MPI_COMM_WORLD, &size);

    if (size < 2) {
        printf("Run with at least 2 processes!\n");
        MPI_Finalize();
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

**Output:**

```
Process 0 sent data: 42
Process 1 received data: 42
```
This simple program shows two processes exchanging data directly.


#### **Collective Communication: Group Operations**

In contrast to point-to-point, collective operations involve all processes in a communicator.

* **`MPI_Bcast`** – Broadcasts a message from one process (root) to all others.
* **`MPI_Reduce`** – Combines values from all processes using an operation (e.g., sum, max) and delivers the result to one process.
* **`MPI_Allreduce`** – Like `MPI_Reduce`, but distributes the result to *all* processes.

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

---

### **2.3 Common Pitfalls: Deadlock**

A **deadlock** occurs when processes wait indefinitely for messages that never arrive.

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

**Fix:** Use non-blocking communication (`MPI_Isend`, `MPI_Irecv`) or adjust the order of operations.

---

### **2.4 Advantages and Disadvantages of MPI**

**Advantages:**

* Scales efficiently to thousands of nodes.
* Offers fine-grained control over communication.
* Works on nearly any distributed system.

**Disadvantages:**

* Communication must be managed manually.
* Deadlocks and synchronization bugs are common.
* Debugging distributed systems can be challenging.

---

**Summary:**
MPI is the backbone of distributed parallel computing. By mastering its core concepts—communicators, ranks, and message passing—you can write efficient programs that scale across large clusters. The next chapters will explore non-blocking communication and advanced collective operations that make MPI even more powerful.




---

Absolutely! To strengthen the practical learning part of your chapter, you can add several **hands-on MPI examples** that illustrate both task and data parallelism in real computations. Here’s how you could expand your section with **practical coding exercises and demonstrations** that fit nicely into your course narrative.

---

## **2.4 Practical MPI Examples: Doing Real Work in Parallel**

These examples demonstrate how multiple MPI processes can collaborate to perform computations. They show how data and tasks can be divided across processes to exploit parallel power effectively.

---

### **Example 1: Parallel Sum of an Array (Data Parallelism)**

Each process gets a chunk of the array, computes a local sum, and then all local sums are combined into a global result using `MPI_Reduce`.

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

**Concepts illustrated:**

* Data parallelism using `MPI_Scatter` and `MPI_Reduce`
* Local computation followed by a global aggregation

**Expected Output:**

```
Global sum = 136
```

---

### **Example 2: Distributed Computation of Pi (Task Parallelism)**

Each process calculates a partial sum of the Leibniz series for Pi, and the results are combined at the end.

```c
#include <mpi.h>
#include <stdio.h>

int main(int argc, char** argv) {
    MPI_Init(&argc, &argv);

    int rank, size, n = 1000000;
    MPI_Comm_rank(MPI_COMM_WORLD, &rank);
    MPI_Comm_size(MPI_COMM_WORLD, &size);

    double local_sum = 0.0;
    for (int i = rank; i < n; i += size) {
        double term = (i % 2 == 0 ? 1.0 : -1.0) / (2.0 * i + 1.0);
        local_sum += term;
    }

    double global_sum = 0.0;
    MPI_Reduce(&local_sum, &global_sum, 1, MPI_DOUBLE, MPI_SUM, 0, MPI_COMM_WORLD);

    if (rank == 0) {
        printf("Estimated Pi = %.12f\n", 4.0 * global_sum);
    }

    MPI_Finalize();
    return 0;
}
```

**Concepts illustrated:**

* Each process performs independent work (task parallelism)
* Use of modular arithmetic to distribute tasks
* `MPI_Reduce` used for final combination

---

### **Example 3: Parallel Matrix-Vector Multiplication (Hybrid Parallelism)**

Each process handles one or more rows of a matrix and computes its contribution to the output vector.

```c
#include <mpi.h>
#include <stdio.h>
#include <stdlib.h>

int main(int argc, char** argv) {
    MPI_Init(&argc, &argv);

    int rank, size;
    MPI_Comm_rank(MPI_COMM_WORLD, &rank);
    MPI_Comm_size(MPI_COMM_WORLD, &size);

    int N = 4; // matrix size
    double A[4][4] = {
        {1, 2, 3, 4},
        {5, 6, 7, 8},
        {9, 10, 11, 12},
        {13, 14, 15, 16}
    };
    double x[4] = {1, 1, 1, 1};
    double y_partial = 0, y_total = 0;

    for (int i = rank; i < N; i += size) {
        double sum = 0;
        for (int j = 0; j < N; j++)
            sum += A[i][j] * x[j];
        y_partial += sum;
    }

    MPI_Reduce(&y_partial, &y_total, 1, MPI_DOUBLE, MPI_SUM, 0, MPI_COMM_WORLD);

    if (rank == 0)
        printf("Matrix-vector product result sum = %.2f\n", y_total);

    MPI_Finalize();
    return 0;
}
```

**Concepts illustrated:**

* Each process handles a distinct set of rows (**data decomposition**)
* Independent computations (**task parallelism**) per row
* Results combined with reduction (**hybrid pattern**)

---

### **Example 4: Master–Worker Model (Task Scheduling)**

This shows **task parallelism** where rank 0 acts as the **master**, distributing work to worker processes and collecting results.

```c
#include <mpi.h>
#include <stdio.h>

int main(int argc, char** argv) {
    MPI_Init(&argc, &argv);

    int rank, size;
    MPI_Comm_rank(MPI_COMM_WORLD, &rank);
    MPI_Comm_size(MPI_COMM_WORLD, &size);

    int n_tasks = 8;
    if (rank == 0) {
        for (int task = 1; task <= n_tasks; task++) {
            int worker = task % (size - 1) + 1;
            MPI_Send(&task, 1, MPI_INT, worker, 0, MPI_COMM_WORLD);
        }

        for (int i = 1; i < size; i++) {
            int stop = -1;
            MPI_Send(&stop, 1, MPI_INT, i, 0, MPI_COMM_WORLD);
        }

    } else {
        while (1) {
            int task;
            MPI_Recv(&task, 1, MPI_INT, 0, 0, MPI_COMM_WORLD, MPI_STATUS_IGNORE);
            if (task == -1) break;
            printf("Worker %d processing task %d\n", rank, task);
        }
    }

    MPI_Finalize();
    return 0;
}
```

**Concepts illustrated:**

* Dynamic task assignment
* `MPI_Send`/`MPI_Recv` for master–worker coordination
* Flexible scaling for irregular workloads

---

### ✅ **Suggested Student Exercises**

1. **Modify the Parallel Sum**: Compute both the sum and average of the distributed data.
2. **Monte Carlo Pi Estimation**: Have each process simulate random points to estimate π collaboratively.
3. **Parallel File Processing**: Assign each process to read a different file chunk and count specific words.
4. **Dynamic Load Balancing**: Extend the master-worker model to assign new tasks when workers finish early.

---

These examples make the MPI section much more engaging and give students the ability to *see* and *measure* parallel execution in action.

</br>
</br>
</br>
</br>
</br>