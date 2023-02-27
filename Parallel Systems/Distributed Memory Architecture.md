don't share address space, ex. have two different 0x10000 addresses
	**Distributed Shared Memory (DSM)**
	but can map different discrete memory into one memory space, into a global address space 
	_programmer view_
[[Distributed Memory.canvas]]
	sharing happens via [[Communication Operations]]


![[Drawing 2023-02-24 11.06.40.excalidraw]]


Openshmem

### distributed memory
any communication is overhead, only exists in parallel programming not sequential

```c
local_n = N/P


//loop till current processor
for(i = 0; i < local_n; i++){ 
	c[i] = 0;
}
//processor based
for(i = 0; i < local_n; i++){
	for(j = 0; ;j < m,j++){
		local_c[i] += local_A[i][j] * local_b[j];
	}
}



for(i = 0 ... n){

}
//row based
for(i = 0; i < n; i++){
	for(j = 0; ;j < m,j++){
		c[i] += A[i][j] * b[j];
	}
}
needs reduction
```
less communication, higher performance
partition with as little communication as possible


### shared memory
```c
A[9][9]
P = 3
local_n = N/P = 3;
rank = {0,1,2}
for(i = 0; i < local_n; i++){
	for(j = 0; j < N; j++){
		c[i + rank * local_n] = A[i + local_n * rank][j] * b[j];
	}
}
```

module unload openmpi
module load openshmem UCX
