
# Network Topology
**topology** - physical structure of interconnects
**routing** - transmission of messages over interconnect
**diameter** - maximum node distance, node distances are minimal
**degree** - maximum node out degree, node with most number of neighbors

**bisection bandwidth** *(equal partition)*
	minimum number of edges we need to cut/removed in order to partition a network into two equal size parts
**node/edge connectivity** *(any separations)*
	**node:** minimum number of nodes to remove to partition a network
	**edge:** minimum number of edges to remove to partition a network
	high connectivity indicates high reliability 
**embedding** *(mapping reduction)* 
	given two networks: **G and G'**
	G' is embedded in/on G if and only if there is a 1 to 1 mapping of all edges and nodes. 
	can embed one topology from one with less connectivity to one with more connectivity and complexity
	one way mapping
	embed into the more complex ones with same connectivity
	

##### Desirable traits
- a small diameter to ensure small distances for message transmission, 
- a small node degree to reduce the hardware overhead for the nodes, 
- a large bisection bandwidth to obtain large data throughputs, 
- a large connectivity to ensure reliability of the network, 
- embedding into a large number of networks to ensure flexibility, and 
- easy extendability to a larger number of nodes.


### Common Topologies
![[Pasted image 20230228180037.png]]
hypercube - power of 2 nodes

![[Drawing 2023-01-30 11.36.08.excalidraw|700]]


The **Hamming distance** of two binary words of the same length is defined as the number of bit positions in which their binary representations differ. In a K-dimensional cube, their hamming distance should reflect on 