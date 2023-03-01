
# Openshmem

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

### textbook
page 60 ish

collective routines
shmem barrier
alltoall, broadcast, reduce

### terminal 
when open lotus
```bash
module unload openmpi
module load ucx openshmem
module list
```
vim .bash
```bash
module unload openmpi
```

```Makefile
oshcc -o shmem shemem.c
```

```terminal
srun -n 1680 ./shmem
```





```c
global A = [][]
. 
.


local[][] = global [i * size + rank][i]
p0 has rows [0,10,20,30]
p1 has rows [1,11,21,31]

int main(){
	for(int i = 0; N /P; i++){
		for(int j = 0; N/P; j++){
			for(int p = 0; p < size; p++){
				if(p == me){
					for(int t = 0; t < N; t++){
						remoteB[t] = localb[t][j]	
					}
				}else{
					broadcast(p, remote b sizeof double * N)
				}
				for(int k = 0; k < N; k++){
					localC[i][j] = localA[i][k] * remomteB[k]
				}
			}
		}
	}

}





for(i to N){
	for(j to N/P2D){
		B[i][j] = A[i][j]
	}
}

gather(0, bG, B, sizeof(double) * N/P_2D * N)



```