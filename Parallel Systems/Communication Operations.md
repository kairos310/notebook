
# Message passing programming models
explicitly called by participating processors, transfer directly between local memories.

**point-to-point** - send operation from one processor to another, receive stores into local address space of executing processor. 
**global-communication-operations** - used for larger sets of processors. 

#### single transfer
- sender specifies send buffer and processor rank of receiving receiver. 
- receiver specifies store buffer for the received message as well as rank of sender
#### single broadcast
- sender(root) sends same data block too all processors, all other processors specify receive buffer
- $P_i$ *root* sends to all
- root, data, count, who
#### single accumulation (reduction/reduce)
- $P_{1...n} \rightarrow P_{i}$ (root) root performs operations
- By performing the operation, a given **reduction** operation is applied element by element to the data blocks provided by the processors, and the resulting accumulated data block of the same length is collected at a specific root processor $P_i$
- (+, -, x, max, min, &&,  ||)
#### scatter
- $P_1$ = | x1 | x2 | x3 |
- distribute *array* over all processors, different data for each, broadcast is the same data. 
- separate data block with **rank**
#### gather
- $P_1$ collects all into single *array*
- **concatenates**, received data blocks, buffer must be large enough
#### multi-broadcast/ all-gather
- Each $P_{1}... P_{n}$ sends a broadcast with a full vector
- distributes the same data block to every processor
- A multi-broadcast is a special case of a total exchange in which each processor sends the same message to each of the other processors
- ![[Pasted image 20230228212053.png]]
#### multi-accumulation/ all-reduce
$$P_{1}x_{11}...x_{1n}
P_{2}x_{21}...x_{2n} 
P_{3}x_{31}...x_{3n} ...
$$
$$ \sum_{i=0}^{j}{X_{i,j}}$$
![[Pasted image 20230228212217.png]]
every processor performs a reduction on things it gets from specific ranks
#### total-exchange/ all to all
- similar to a transpose
$$P_{1} X_{11}\rightarrow P_{1} X_n$$
- sends every bit of data to all processors

```c
for i = 0 ... m{
	for j = 0 ... m{
		c[i] += a[i][j] + b[i]
	}
}
```



### Dual operations
![[Pasted image 20230228215419.png]]
Since the same spanning trees can be used, single-broadcast and single-accumulation are dual operations
Can reverse a tree 



step-wise specialization. A total exchange is the most general communication operation, since each processor sends a potentially different message to each other processor



[[Interconnects]]