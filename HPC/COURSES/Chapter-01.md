# High-Performance Computing – Introduction to Parallel Computing

**Course:** High-Performance Computing / Introduction to Parallel Computing</br>
**Instructor:** Dr. Anouar Khaldi

---

### 1. Why We Need Parallel Computing

#### 1.1 The Limits of Sequential Computing

Modern computing problems—AI, simulation, and massive data—demand processing speeds that single CPUs can’t achieve fast enough.

**Example: Weather Forecasting**
A 3D grid with 100 million points and 1000 time steps requires ~400 billion calculations. One 3 GHz CPU would take ~37 hours. With 100 CPUs, that drops to about **22 minutes**.

#### 1.2 The Power Wall

Clock speeds hit physical limits (power, heat) around 2004. Instead of faster cores, CPUs began adding **more cores**.

| Year | CPU Example   | Clock Speed | Cores | Trend              |
| ---- | ------------- | ----------- | ----- | ------------------ |
| 1998 | Pentium II    | 450 MHz     | 1     | Sequential era     |
| 2004 | Pentium 4     | 3.8 GHz     | 1     | Power wall reached |
| 2010 | i7 980X       | 3.3 GHz     | 6     | Start of multicore |
| 2025 | Ryzen 9 7950X | 3.5–5.7 GHz | 16    | Multicore standard |

Instead of increasing frequency, we now increase **parallelism**.

#### 1.3 Applications

* **Scientific Simulation:** climate, physics, molecular dynamics
* **AI:** deep learning, image recognition
* **Data Analytics:** big data, recommendation systems



### 2. Fundamental Laws of Parallel Computing

#### 2.1 Amdahl's Law (Speedup Limit)

$$Speedup(N) = \frac{1}{S + \frac{P}{N}}$$

Where:
* **S** = Sequantial fraction
* **P** = Parallel fraction
* **N** = Number of processors

Even with infinite processors, speedup is limited by S.

**Example:** If 90% of code is parallel (P=0.9) and 10% is sequantial (S=0.1), with 4 processors:
 ```
 Speedup = 1/(0.1 + 0.9/4) ≈ x3.08
 ```
Adding more processors won't overcome that sequantial bottleneck.

#### 2.2 Gustafson's Law (Scalable Workload)

As problem size grows, parallelism scales better:

$$Speedup = S + P \times N$$

**Example:** Same 10% sequantial, 90% parallel code. With 4 processors:
```
Speedup = 0.1 + 0.9×4 = x3.7
```
Unlike Amdahl's, efficiency improves as we increase both problem size and processors together.



### 3. Parallel Computer Architectures

#### 3.1 Shared Memory Systems (SMP)

All processors share a **single main memory** and communicate via a common bus.

```
          ┌──────────────┐
          │  Shared RAM  │
          └──────┬───────┘
                 │
 ┌──────────┬────┴────┬──────────┐
 │          │         │          │
 ▼          ▼         ▼          ▼
CPU1       CPU2      CPU3       CPU4
```

**Characteristics:**

* All CPUs can access any memory address.
* Communication occurs via shared variables.
* Synchronization is needed to avoid data races.

**Advantages:**

* Easy to program (common memory model)
* Fast inter-core communication

**Disadvantages:**

* Memory contention increases with more CPUs
* Limited scalability due to shared bus

**Use case:** desktops, small servers
=>


#### 3.2 Distributed Memory Systems (Clusters)

Each node has its **own private memory**, and nodes communicate through a network.

```
┌─────────────┐     ┌─────────────┐     ┌─────────────┐
│ Node 1      │     │ Node 2      │     │ Node 3      │
│ ┌─────────┐ │     │ ┌─────────┐ │     │ ┌─────────┐ │
│ │ CPU+RAM │ │◄───►│ │ CPU+RAM │ │◄───►│ │ CPU+RAM │ │
│ └─────────┘ │     │ └─────────┘ │     │ └─────────┘ │
└─────────────┘     └─────────────┘     └─────────────┘
       ▲                   ▲                   ▲
       │                   │                   │
       └─── Network Interconnect (Ethernet) ───┘
```

**Characteristics:**

* Each node manages its own memory.
* Nodes communicate using **message passing (MPI)**.

**Advantages:**

* Scales to thousands of nodes
* Fault isolation (failure of one node doesn’t stop others)

**Disadvantages:**

* Slower communication (network latency)
* Harder programming (explicit data transfer)

**Use case:** supercomputers, cloud clusters



#### 3.3 NUMA (Non-Uniform Memory Access)

A hybrid between SMP and Clusters. CPUs have **local memory** but can also access other sockets’ memory (slower).

```
┌─────────────────────────────┐        ┌─────────────────────────────┐
│          Socket 1           │        │          Socket 2           │
│  ┌─────────┐   ┌─────────┐  │        │ ┌─────────┐    ┌─────────┐  │
│  │  CPU1   │   │  CPU2   │  │        │ │  CPU3   │    │  CPU4   │  │
│  └────┬────┘   └────┬────┘  │        │ └────┬────┘    └────┬────┘  │
│       ▼             ▼       │        │      ▼              ▼       │
│   ┌──────────────────────┐  │        │  ┌──────────────────────┐   │
│   │     Local RAM 1      │◄─┼────────┼─►│     Local RAM 2      │   │
│   └──────────────────────┘  │        │  └──────────────────────┘   │
└─────────────────────────────┘        └─────────────────────────────┘
                ▲                                      ▲
                └── Inter-socket Link (Slower Access) ─┘
```

**Characteristics:**

* Each socket has its own local memory bank.
* Remote memory access is slower but still possible.

**Advantages:**

* Balance between SMP simplicity and cluster scalability
* Fast local access, moderate remote access

**Disadvantages:**

* Complex thread scheduling and memory locality
* Performance drops if threads frequently use remote memory

**Use case:** modern multi-socket servers (AMD EPYC, Xeon)



### 4. Summary of Architectures

| Type    | Memory      | Communication           | Scalability | Example             |
| ------- | ----------- | ----------------------- | ----------- | ------------------- |
| SMP     | Shared      | Shared bus              | Low–Medium  | Desktop/workstation |
| Cluster | Distributed | Network (MPI)           | Very High   | Supercomputer       |
| NUMA    | Semi-shared | High-speed interconnect | High        | Multi-socket server |



### 5. Trends in Modern Parallel Computing

* **Heterogeneous computing:** CPUs + GPUs + TPUs
* **Cloud parallelism:** distributed jobs across thousands of machines
* **Energy-aware scheduling:** optimizing for performance-per-watt
