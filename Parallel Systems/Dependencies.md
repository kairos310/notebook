#### Flow dependency (RAW)
R1 <- R2 + R3
R5 <- R1 + R4
R5 needs to wait for R1, 
also *true* dependency



#### Anti dependency (WAR)
R1 <- R2 + R3
R2 <- R4 + R5
R2 needs to be saved before its modified

#### Output dependency (WAW)
R1 <- R2 + R3
R1 <- R4 + R5
with sequential execution, make sure last write is the one that persists


![[Drawing 2023-02-17 11.11.11.excalidraw]]

Cannot parallelize this
```c
//antidependency
for (i = 1; i < N; i++){
	a[i - 1] = b[i];
	c[i] = a[i];
}

//flow dependency
for(i = 1; i < N; i++){
	a[i] = b[i];
	c[i] = a[i - 1];
}

for(i = 1; i < N; i++){
	a[i] = b[i];
	a[i - 1] = c[i];
}

```
![[Drawing 2023-02-17 11.35.17.excalidraw]]



### Processor consistency models / Total store ordering(PC, TSO)
relax $W\rightarrow R$  orderings to hide latency
### Partial Store Ordering 
relax $W \rightarrow W$ and  $W\rightarrow R$  
write operations can be complete if there's no output dependency 