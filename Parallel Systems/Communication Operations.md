
### message passing programming models
explicitly called by participating processors, transfer directly between local memories.

**point-to-point** - send operation from one processor to another, receive stores into local address space of executing processor. 
**global-communication-operations** - used for larger sets of processors. 

#### single transfer
- sender  specifies send buffer and processor rank of receiving receiver. 
- receiver specifies store buffer for the received message as well as rank of sender
#### single broadcast
- sender(root) sends same data block too all processors, all other processors specify receive buffer
- $P_i$ *root* sends to all
- root, data, count, who
#### single accumulation (reduction/reduce)
- $P_{1...n} \rightarrow P_{i}$ (root) root performs operations
- (+, -, x, max, min, &&,  ||)
#### scatter
- $P_1$ = | x1 | x2 | x3 |
- distribute *array* over all processors, different data for each, broadcast is the same data. 
#### gather
- $P_1$ collects all into single *array*
#### multi-broadcast/ all-gather
- Each $P_{1}... P_{n}$ sends a broadcast with a full vector
#### multi-accumulation/ all-reduce
$$P_{1}x_{11}...x_{1n}
P_{2}x_{21}...x_{2n} 
P_{3}x_{31}...x_{3n} ...
$$
$$ \sum_{i=0}^{j}{X_{i,j}}$$
$$$$
#### total-exchange/ all to all
- similar to a transpose
$$P_{1} X_{11}\rightarrow P_{1} X_n$$


```c
for i = 0 ... m{
	for j = 0 ... m{
		c[i] += a[i][j] + b[i]
	}
}
```
# Interconnects
---
<div style="padding: 1em; background:#181818">links all all Compute Nodes together
*eg. ethernet cable, InfiniBand switch*</div>

### Static interconnect
all nodes connected to each other, all one hop away from each other

### Dynamic interconnect
some nodes aren't connected to each other and there are branches and switches
eg. InfiniBand

![[Drawing 2023-02-03 11.08.04.excalidraw]]
Fat tree networks

# Topology
**topology** - physical structure of interconnects
**routing** - transmission of messages over interconnect
**diameter** - maximum distance between any two nodes
**degree** - maximum out degree (how many nodes one particular node is connected to) of any node

**bisection bandwidth** *(equal partition)*
	minimum number of edges we need to cut/removed in order to partition a network into two equal size parts
**node/edge connectivity** *(any separations)*
	**node:** minimum number of nodes to remove to partition a network
	**edge:** minimum number of edges to remove to partition a network

**embedding** *(mapping reduction)* 
	given two networks: **G and G'**
	G' is embedded in/on G if and only if there is a 1 to 1 mapping of all edges and nodes. 
	can embed one topology from one with less connectivity to one with more connectivity and complexity
	one way mapping

### Common Topologies

|                     | fully connected graph | linear array | ring | mesh - 2D       | hypercube | torus | 
| ------------------- | --------------------- | ------------ | ---- | --------------- | --------- | ----- |
| diameter            | 1                     | n - 1        | $\frac{n}{2}$  | $2\sqrt{n - 1}$ |           |       |
| degree              | n - 1                   | 2            | 2    | 4               |           |       |
| edge connectivity   | n - 1                 | 1            | 2    | 2               |           |       |
| bisection bandwidth | $\frac{n}{2}^2$       | 1            | 2    | $\sqrt{n}$      |           |       |


hypercube - power of 2 nodes

![[Drawing 2023-01-30 11.36.08.excalidraw]]



## MPI
message passing interface
negotiates send and receive commands
sort of like an API to pass information to one another
	openMPI
	MPICH
	MVAPICH


### PGAS
partitioned GAS
doesn't need the node to open receive command, get node can direrctly retrieve data
	openSHMEM
	UPC
	ARMCI

### FSB
front side bus

