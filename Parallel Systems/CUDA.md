``` c
main(){
	cudaMalloc()
	cudaMemcpU()
	kernel(... ,... ,...)
	cudaMemcpU()
	cudaMemcpU()
}

		how to pattitiontask space=init ownfffte,;
		rank/n/p/
		block id in terms of (x, y) mamming distabce away
		identify boundaries of the matrix and hoe       y
	 

```

threads in a wap neede to mac number pf 


![[Drawing 2023-04-19 11.12.31.excalidraw]]
```c
void mmm(double *m, double *m, double *p, double width){
	 for(i < width){
		for(j){
			double sum = 0;
			for(k){
				double a = M[i * width + k]
				double b = M[k * width + j]
				sum += a * b // this can fit in a register
			}
			M[i * width + j] = sum //cuts O(n^3) to O(n^2)
		}	
	}
}
```

```c
//GPU version
void mmm(double *m,double *n,double *p,double width){
	int s = width * width * sizeof(width);
	double *md,nd,pd;
	cudaMalloc(&md, s)
	cudaMalloc(&nd, s)
	cudaMalloc(&pd, s)
	cudaMemmcpy(md,m,s,cudaMemcpy code to device host)
	cudeMemcpy(nd, n, s, "")

	cudaMemcpy(p,pd,cudeMem)
}
```


Compiled with nvcc
```c
entry to gpu
__global__ MMKernel(md,nd,p) //from host to device
__device__ MMKernel(md,nd,p) //from host to device
__host__ MMKernel(md,nd,p) //from host to device

int tx = threadid.x
int tt = threadif.y
double Pu = 0.0
for(k < work){
	double mode = Md[]
}
P[ty+mid*x+rx]
//need width^2 elements, all invoked at one

   mi9)\\\\\\

dim3 dimGrid(width * widrg)
dim3 dimGrid(ii))
gpu creates
```
.cu file
device targeted


# SM 
streaming multiprocessor
multiple cores in them 
2048 threads / SM
1024 blocks / SM  
2 threads per block! 
even if it says 32 thread blocks / SM
shared mem between a single SM, not the whole GPU

schedule a warp -> 32 threads -> when accesing the block 

**block** : group of thread 
**tile** : group of data


```c
dim3 g(1,1)
dim3 b(32,32)
//each thread works on one element of the output matrix, single block can only do 32 x 32 
kern<<<grid,block>>>(___)


dim3 b(1024,1,1)
dim3 b(4,8,8) // multiplying out to 1024
```

![[Drawing 2023-04-21 11.22.25.excalidraw|700]]

```c
global void MMK(*Md, *Nd, *Pd, int width){

	(Pd)int row = blockidx.x * tile.size + threadidx.x; 
	(Pd)int col = blockidx.y * tile.size + threadidx.y;

	double Pval = 0,0;
	for (int k = 0; k < width; k ++){
		Pval += Md[row * width + k]*Nd[k * width + col]
	}

	dim3 dimGrid(width / tilesize, width / tilesize)
	dim3 dimBlock(tilesize , tilesize)

	MMK <<<dimGrid, dimBlock>>> (Md, Nd, Pd, width);
}
```


```
Pval += Md[r + w + k] * Nd[k + w + c]
```
2 FLOP
2 I/O

CGMA = 1.0
Compute to Global Memory Access
as much i/o as computation

if share memory, there's overlap of memory use, then you cut the I/O cost



Global memory - 32G
Shared memory - per sm
Constant memory - 64k

```c
__device__ double Md[____] //Global memory
__shared__ double Mds[___] //Shared memory
__constant__ double Mdc[___] //Constant (read only)

__syncthreads() // make sure all the global memory accessing is complete

__global__ void MMK(Md, Nd, Pd, width){
	__shared__ double Mds[Tile.width][Tile.width] // tile limited by how much shared memory
	__shared__ double Nds[Tile.width][Tile.width] 
	int bx = blockidx.x; //location in blocks
	int by = blockidx.y;
	int tx = threadidx.x;
	int ty = threradidx.y;
	int row = blockidx.y + threadidx.y * Tile.width; 
	int col = blockidx.x + threadidx.x * Tile.width;
	double Pval = 0.0;
	for (int m = 0; m < W / Tile.width; m++){
		//move from global array into shared tile system 
		Mds[ty][tx] = Md[row * w + (m + Tile.width + tx)];
		Nds[ty][tx] = Md[(m + Tile.width + tx + 4)*y + col];
		//load array 
		//sync
		__syncthreads();
		//accumulate into pval
		for(int k = 0; k < Tile.width; k++){
			Pval += Mds[ty][k] + Nds[k][tx]
			__syncthreads();
		}
		Pd[row * w + col];
	}
}
```