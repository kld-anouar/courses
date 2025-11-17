### MPI Homework: Distributed Maximum Finder</br>
**Instructor:** Dr. Anouar Khaldi
### Objective
Implement a distributed algorithm using **Point-to-Point** and **Collective** communication to find the maximum value across all processes. Run with **4 processes** (`mpirun -np 4`).


### Tasks

#### Task 1: Initialization
Initialize `my_value` in every process ($P_i$):
$$\text{my\_value} = 100 + \text{rank}$$


#### Task 2: P2P Exchange (P0 $\leftrightarrow$ P1)
$P_0$ and $P_1$ must exchange their `my_value` using `MPI_Send` and `MPI_Recv`.

**Deadlock Prevention:**
* $P_0$ must **Send** then **Receive**.
* $P_1$ must **Receive** then **Send**.

Store the received value in `received_value`.

$P_2$ and $P_3$ skip this step.


#### Task 3: Local Maximum Calculation
Set `local_max` for each process:

* If $P_0$ or $P_1$:
    $$\text{local\_max} = \max(\text{my\_value}, \text{received\_value})$$
* If $P_2$ or $P_3$:
    $$\text{local\_max} = \text{my\_value}$$


#### Task 4: Collective Reduction (Global Maximum)
Use **`MPI_Reduce`** with the **`MPI_MAX`** operation to find the true global maximum of all `local_max` values. The result must be stored in `global_max` on $P_0$.

**Output:** $P_0$ must print: `"The Global Maximum is: 103"`

</br>
</br>
</br>
</br>
</br>