# Distributed Memory Architecture

##### DMM 
Distributed Memory Machines
consists of [[Compute Nodes]] and Interconnects [[Communication Operations]]
[[Interconnects]]

##### Point to point connections 
- a node connected to a fixed set of other nodes represented as a graph
- restrict parallel programming, topology determines data exchange.


##### DMA controller
- decoupling of *processor* operations from *communication* operations
- direct memory access without processor action, parallel
- processors issue send operation to **DMA**
- only communicate efficiently with neighbors.
- *routers* can be added, every node connects to it and this decreases time for non neighboring nodes, almost equal

##### Clusters
based on standard computers, entire thing programmed as a single unit, standard message passing models.
##### Distributed Systems
similar to clusters, but don't have singular operating system, special job scheduler
##### Grids
Amazon Cloud, Microsoft Azure.


# Shared Memory Architecture
##### SMM (shared memory machines)
global memory, shared variables
can create **race conditions** at [[Critical section]] 

##### pro
The existence of a global memory is a significant advantage, since communication via shared variables is easy and since no data replication is necessary as it is sometimes the case for DMMs.
##### con
The realization of SMMs requires a larger effort, in particular because the interconnection network must provide fast access to the global memory for each processor. This can be ensured for a small number of processors, but scaling beyond a few dozen processors is difficult

