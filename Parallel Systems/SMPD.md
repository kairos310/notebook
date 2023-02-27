**Single program Multiple Data**
	distributed memory computation
		distributed data, no access to shared data, if data needed from another core, need to explicitly ask for it
	asynchronous execution
	**rank** - 
	**size** - number of processes
	everyone does the same thing on different portions of the data

Global sum on rank 0 
## collective operations 
#rem
**point to point fence** - waits until one is done before going to next phase
**collective fence** - waits until everyone is pass the fence before going to next phase
**barrier** - everyone needs to call barrier, everyone reaches the barrier before the program continues execution. bad for performance, only for reporting. 
**reduction** - 

``` c
for (int i = 0; i < size; i++){
	if(rank = i){
		printf("gs: ",rank, local_sum)
	}
	barrier();
}
```

``` c
//init
//size = num processors, srun, sbatch, -n
int size = 10;
//rank = unique id for each process
int rank = 4;
int N = 1000;

local_size = N / size                       // 100
local_base = rank * local size              // 400
local_upper = (rank + 1) * (local_size - 1) // 499
local_sum = 0.0 

for (int i = local_base; i < local_upper; i++){
	local_sum += x[i] + y[i];
}

reduce(&local_sum, &global_su, 0, sum);
if(rank = 0){
	printf("local sum %d", gloabal_sum);
}
```


OpenMP
pthreads - fork-join model

client-server 
like web browsers 
