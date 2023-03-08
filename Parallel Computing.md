
1. parallel decomposition, breaking down into same task but smaller
2. map tasks into threads 
3. map into physical cores

### Classification of computers
[[Flynn's taxonomy]]

### Memory Distribution
[[Memory Architecture]]

cache,  dependencies , network, coherency , consistency , 

---
# LAB STUFF 

### Don't run on the login (head) nodes

system call -> interrupts the operating system, takes hundreds of cycles
TSC constant clock tick 
needs to calibrate how many ticks per second
```
gcc -g -c -o lab1.o lab1.c
```


```
srun -n 1(how many threads you need) ./lab1 300(arguments for lab)
```
or 
### to open a new node
```
salloc -n 1
//logs you into a compute node

./lab1 300
```


put functions on the top of the file

#### lab tips
*Pass pointers into arrays don't pass full arrays with sizes, can put parameter with sizes*

When malloc do an nxn malloc, don't do individual mallocs for each inner array
index = i x n + j

init2d returns void
```c
takes **, num_rows, num_cols, base_value

linearization of 2D space
for i
	for j
		a[(i*n) + j]
instead of a[i][j]
```

### Makefile tags
```c
// force no warnings 
-o
// debugger on
-g

//optimization
-O1, runs a little faster
-O3 full 

//vectorization
-msse4.2

// change compiler
clang

// compiles into assembly
-S 

```

Compilers are designed to be conservative, designed to work with all cases. Optimization might do something that seems legal and but could change something fundementally. 
More optimization would decrease accuracy

### LAB 2
cd/home/comp380/examples
```c
gcc -g -fopenmp -o openmp openmp.o

./openmp [loop iterations] [n processes]
./openmp 10 10

#include <stdio.h>
#include <stdlib.h>
#include "omp.h"

//thread usage
omp_set_num_threads(atoi(argv[2]))
omp_set_num_threads(1)

#pragma omp parallel{
	// this section is in parallel
	int thread_num = omp_get_thread_num(); 
	int threads = omp_get_threads();
	printf("")
}

#pragma omp parallel for
for(){

}

#pragma omp parallel for schedule
{
	//grab 3 iterations of the loop
}

//timer
double seconds = omp_get_wtime();
double final = omp_get_wtime();

```
Graph these over number of cores

```c
double x, pi, sum, step;
step = 1.0 / nsteps
omp makes sure iterator is a private variable 

#pragma omp parallel for private(x) reduction(x)
// cannot use the same x
// sum needs to be used by all
for(i = 0; i < n; i++){
	x = somthingg
	sum = sum + x
}
//lots of data dependency



```

**reduction:** reduce something in dimensionality, calculates a property in a lower dimension, must have some base case (identity, 0 for addition, 1 for multiplication), has some sort of recursive notion
	what are the tasks that need to be done 
	could be a single calculation of c ij
	map number of cores to number of tasks
	what are the needs for accessing shared data, addressing dependencies
	static vs dynamic task decomposition
**broadcast:** opposite

granularity: 
	fine grained: small tasks, many tasks, load imbalance can is washed out, better work distribution
	coarse grained: larger tasks, good locality properties, less coordination


12 tables
	4 sizes 100,000 3000 5000 
	3 tables 
get the times, the speedups
| n   | ijk | ikj | jki | mxm2 (with transpose) |
| --- | --- | --- | --- | --------------------- |
| 1   |     |     |     |                       |
| 2   |     |     |     |                       |
| 4   |     |     |     |                       |
| 8   |     |     |     |                       |

omp_wget_time() time stamp as double, don't need to reinitialize

### LAB 3
n-body simulation, big shared array one processor
parachute
<div style="border-radius:0.5em;padding:1em; background: linear-gradient(135deg, hsla(0, 0%, 13%, 1) 0%, hsla(233, 42%, 23%, 1) 100%);">
distribute memory over multiple cores
<br>matrix operations are faster 
<br>can use an adjacency matrix</div>


```c
#include <shmem.h>

int main(){
	static
	shmem_init();
	rank = shmem_my_me();
	receiver = 1% shmem_n_pes();
	
	if(rank = 0){
		int arc = 33
		shmem_put(&targ, &src, 1, receiver) //the put blocks
	}
	
	shmem_barrier_all(); //waits until 33 is placed in memory
	
	if(rank == receiver){
		print()
	}
	
	shmem_finalize();
	return

}
```

```sh
#!/bin/bash
#SBATCH -n 960
#SBATCH -N 20
#SBATCH --ntasks-per-node=48

srun --mpi=pmi2 -o  ~path args 

```

can add pointer to the global_s struct
use eprintf to debug
don't use arrays of size N
	use many arrays of size N/P
copy main over to lab 3
two arrays, bodies and next
copy next to bodies when done computation
Ex. 1000 bodies and 2 processors
[0. ..  499], [500 - 999]
this is actually 0

shemem_calloc - collective (everyone calls it at the same time, waits until everyone gets there) zeroes out the zeroes, pointer to the portion of the array,
shemem _free_

shmem_get(dest, src, size, pe) to read things

shememinit
getmbp
git mymp
getbarrier every timestep
calloc to allocate arrays that's shared - bodies
next - local private
variations of put and get, variations bring performance

**basics**
calloc
barrier
getmem


calculate it's own forces, add to next
reduction on all next
move next to bodies 
```c
for(ts = 0; ts < g.nsteps; ts++){
	computeforce(); // bodies -> nex[]
	barrier()
	memcpy() // next -> bodies
	barrier()
}
```

```
bodies
530 / 250 = 2;
530 % 250 = 30;
shmem_getmem(&temp, &bodies[30], sizeof(body_t) * nbody, rank)

```

![[Drawing 2023-03-06 11.05.36.excalidraw|600]]


---

# [[Dependencies]]
### Cost
$$Cost(n)= C_{p}(n) = processor\ count \times parallel\ execution\ time = p \cdot T_{p}(n)$$

### Speedup 
$$Speedup=S_{p}(n)=\frac{T^*(n)}{T_{p}(n)}=\frac{time\ sequential}{time\ parallel}$$
### Efficiency
$$E_p(n)=\frac{T^*(n)}{C_{p}(n)}=\frac{time\ sequential}{parallel\ execution\ time}=\frac{speedup}{processor\ count}$$
### Communication times 
$t_{comm}$ =communication time
$t_s$ = startup cost
$t_b$ = transfer cost 
$T_o$ = overhead
$T_s$ = sequential time
$T_p$ = parallel time
$$t_{comm}=t_{s}+Bt_b$$
$$T_{o} = p T_{p}-T_{s}$$

---
# [[Memory Layout]]

# [[RAM]] Types
Static vs Dynamic 
# [[Cache]]
- SRAM
- levels
- data and instruction
- tags (TSO)
- dirty
- #coherency
- associativity
	- fully
	- direct mapped
	- set
- random vs sequential
- TLB pages, cache lines

# Threads
different threads have separate stacks but have a shared heap

### SMT - simultaneous multithreading 
#hyperthreading
separate program counters, separate registers, single core, everything else is the same
makes context switching very fast, works when different threads are running in entirely different tasks. 
can save power by running slower hz

MISP - do all operation in registers
x86 - 





![[Code formatting]]


# [[Virtual Address]]
- mapping to pages in physical memory
- index the page table
- cached to TLB
# Test
![[Review]]