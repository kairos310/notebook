
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


[[Interconnects]]