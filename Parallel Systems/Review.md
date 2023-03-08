# Quizzes
---
In the first commodity hardware system, the role of the Northbridge and Southbridge. 
- Northbridge connects the CPU and RAM through the FSB. [[Memory Layout]]
- Southbridge connects the Northbridge with external i/o
	- USB, PCIE, SATA, 
- limited by the FSB, singular RAM port. 


#### *Alternative solutions to the first commodity hardware system.* 
External controllers
Integrated memory controllers

advantage: 
	CPU to memory connected no longer limited by FSB bandwidth
disadvantage:
	**NUMA** (Non Uniform Memory Architecture)
	- memory attached to a different processor, memory retrieval is slowed depending on how many levels of interconnects away it is

##### *DRAM vs SRAM*
[[RAM]]
**SRAM**
1. more complex, 6 transistors
2. more expensive 
faster, not limited b discharge speeds, needs power line and not refresh

**Four problems with DRAM**
1. needs refreshing every 64 ms
	- can use up to 50% memory
2. information from cell needs to be amplified
3. reading cell discharges cell, operation is followed by feeding output back in
4. charging and draining is not instantaneous

---
##### Working Set
site of the memory reload for a program, the page

##### Exclusive cache
only one cache gets the data, when not used, data is evicted up the hierarchy 
[[Cache]]
##### Inclusive Cache

# Test Study guide

Ch 1 - skim  
Ch 2:  
- [ ]  2.1 - skim
- [x] 2.2 - [[Flynn's taxonomy]] 
- [x]   2.3 -  Memory Organization of Parallel Computers
	- [x] [[Memory Architecture]]
- [x]  2.4 - 2.4.2 read, otherwise skim
	- [x] [[Threads]] 
		- [x] coarse
		- [x] fine
- [x]  2.5-2.5.2 master
	- [ ] [[Interconnects]] 
	- [x] [[Network Topology]] 
- [ ]  2.7 — yass queen
	- [x] [[Cache]]
	- [ ] [[Cache Associativity]]
	- [ ] [[Write Behaviour]]
	- [x] [[Cache Consistency]]
		- [ ] Relaxed Consistency
		- [ ] processor consistency models
		- [ ] partial store ordering
Ch 3:  
- [ ]   3.0-3.1 - skim
	- [ ] [[Parallel Computing]]
	- [x] [[SPMD]]
	- [x] Models
		- Machine
			- lowest level of abstraction, registers and input output buffers 
		- Architectural
			- network of parallel platforms, memory platforms, sync async processing, execution mode *SIMD* *MIMD*
		- Computational 
			- The complexity of an algorithm should reflect the performance on a real computer. The RAM model is suitable for theoretical performance prediction although real computers have a much more diverse and complex architecture.
		- Programming
			- a parallel computing system in terms of the semantics of the programming language or programming environment
		+ Task
			+ executed in parallel mapped to processors, control flows also called *threads*			
- [ ]   3.2-3.3.2 - mastery
	- [x] Parallelization of programs
		- **Decomposition** through shared memory or message passing algorithms
			- The goal of task decomposition is therefore to generate enough tasks to keep all cores busy at all times during program execution. 
			- The tasks should contain enough computations such that the task execution time is large compared to the scheduling and mapping time required to bring the task to execution.
			- The computation time of a task is also referred to as *granularity*
		- **Assignment of tasks to processes or threads** 
			- The number of processes or threads does not necessarily need to be the same as the number of physical processors or cores, but often the same number is used. 
			- **scheduling algorithm**
				- a method to determine an efficient execution order for a set of tasks of a given duration on a given set of execution units
				- dependencies between the tasks, leading to precedence constraints
			- assign the tasks for good load *balancing*
		- **Mapping of processes**
			- to physical cores
			- If less cores than threads are available, multiple threads must be mapped to a single core
			- ![[Pasted image 20230228210039.png]]
- [ ]   3.3.3 - skim
- [ ]   3.3.4-3.3.5 are my jam, but you don’t need to know them (skim)
- [ ]   3.3.6.x - skip master/worker otherwise read
	- [x] Parallel programming patterns
		- *Fork-join* (The *spawn* and *exit*) for openMP
			- these operations provided by message-passing systems like MPI-2, see Sect. 5, provide a similar action pattern as fork-join.)
		- *parbegin*–*parend*
			- aka *cobegin - cooend*
			- allows the specification of a sequence of statements, including function calls, to be executed by a set of processors in parallel.
			- a set of threads is created and the statements of the construct are assigned to these threads for execution
		- [[Flynn's taxonomy]]
		- SIMD SPMD
			- single instruction/ program multiple data
			- SIMD - same instruction applied to a large set of data, eg. graphics 
			- SPMD - different threads work asynchronously, no implicit synchronization 
		- MPMD
			- Client Server model 
				- parallelism on server side, 
			- Task pools
				- fixed number of threads created in the beginning
				- 
		- **Pipelining**
			- special form of functional decomposition where the pipeline threads process the computations of an application algorithm one after another.
- [x]   3.4.x - master (3.4.3 is advanced, but may help if you like math)
	- [x] [[SPMD]]
- [x]   3.5 - master
	- [x] [[Data Distribution]]
- [ ]   3.6 - master
	- [x] **Shared Variables**
		- Each thread can access **shared** data in the global memory. Such shared data can be stored in shared variables which can be accessed as normal variables
		- .A thread may also have private data stored in **private** variables, which cannot be accessed by other threads.
		- Usually, a **sequentialization** is performed such that concurrent accesses are done one after another.
		- To ensure that T2 reads the variable not before T1 has written the appropriate data, a **synchronization** operation is used. T1 stores the data into the shared variable before the corresponding synchronization point and T2 reads the data after the **synchronization** point.
		- The term **race condition** describes the effect that the result of a parallel execution of a program part by multiple execution units depends on the order in which the statements of the program part are executed by the different units
		- This may lead to **nondeterministic behavior**, since, depending on the execution order, different results are possible, and the exact outcome cannot be predicted.
		- Program parts in which concurrent accesses to shared variables by multiple threads may occur, thus holding the danger of the occurrence of inconsistent values, are called **critical sections.**
		- An error-free execution can be ensured by letting only one thread at a time execute a critical section. This is called **mutual exclusion.**
	- [x] [[Communication Operations]]
		- The corresponding programming models are therefore called **message passing programming** models.
		- A **send operation** sends a data block from the local address space of the executing processor to another processor as specified by the operation. 
		- A **receive operation** receives a data block from another processor and stores it in the local address space of the executing processor. This kind of data exchange is also called *point-to-point* communication
- [ ]   3.7 - skim (really good stuff in here, mostly not on test)
- [ ]   3.8 - skim (really good stuff in here, mostly not on test)