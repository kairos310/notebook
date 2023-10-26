groups of processors that do a bin of tasks
like decimation in image processing
can barrier only for a singular group       
**intra communicators** communicate between notes in groups
**inter communicators** communicates data between groups

MPI1 static process model
- all processes are participating, start in 1 process group
- com rank and size comes from world and groups

MPI2 dynamic process creation
allows assini

openshmem can access anything in the symmetric heap
mpi target has to define region, exposed region o

one sided communication doesn't bother the CPU, two di;
async
flexible
comoms without interruptin 


## False Sharing
accessing the same page, cache takes the whole page, false dependency because changes different things, but it's on the same page, be aware of what page the data is on

## Speculative prefetching
guessing what comes next, based on column, row wise

# MPI One Sided Communication
Non blocking communication
| **Two sided** | **One Sided**  |
| ------------- | -------------- |
| MPI Send      | MPI_Put        |
| MPI_Receive   | MPI_Receive    |
| MPI_Reduce    | MPI_Accumulate |

### One sided
##### initiator
##### receiver 
RDMA
Remote Direct Memory Access
supplied by the NIC, allows atomic operations



## Window
<div class="def">block of memory exposed for one sided operations
<br> MPI - need to explicitly say that small region is exposed, don't say whole memory is exposed 
</div>

``` c 
MPI_Win_Create(*base, size, sizeof(int), info, MPI_COMM_WORLD, window);
//not everyone needs the same base, not symmetric

MPI_Put()
MPI_Get()
MPI_Accumulate() //uses atomic operations, race condition free, limited to 64 bit values.

```

## Collectives and Syncs
```C
//collective
//any RMA is gauranteed to complete before this point, global sync
//Strict Sync
MPI_Win_Fence()


//initiator
MPI_Win_Start()
//do stuff
//access epoch
//gauranteed that everything in here is complete before leaving, all your communications are done
//MPI_PUT()
MPI_Win_Wait()

//receiver
MPI_Win_Post()
//do stuff
//exposure epoch
//nobody else is still going stuff with my memory
MPI_Win_Wait()
```