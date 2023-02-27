 *A subset of data in main memory*
Doesn't store words, operates in terms of cache lines
	typically $2^8$
Cache lines == Cache blocks
CPU does not deal with cache

1. check the cache, cache hit
2. bring into cache, cache miss

~ *maximize hit rate*

#### Working Set
the set of data currently worked on
in systems running multiple processes at the same time, the working set is the sum of the sizes of all the individual processes and the kernel

### AMAT
Average Memory Access Time
#rem
$(miss\ rate \times\ total\ access \times miss\ cost) + (hit\ rate \times\ total\ access \times hit\ cost)$


#### Locality
#spatial_locality  - same code or data gets used back to back, like in loops
**Temporal Locality** - how frequent the same code is used
```
for(i = 0; i < N; i++){
	sum += a[i];
}
// accessed i 3N + 1 times
// lots of temporal locality

for(i = 2; i < N; i++){
	a[i] = a[i-1] + a[i-2]
}
// lots of spatial locality
```

### Cache hierarchy 
![[Pasted image 20230201184710.png]]
*minimum configuration*
all load and stores go through the cache, the FSB connects the cache and the main memory

![[Pasted image 20230201184841.png]]
*modern systems*
typically each modern core has it's own copy of their hardware resources, these are separated into instruction (i) and data(d) caches. 

### Tags

Address remains unchanged
	Tag
	Cache position 
	Offset
![[Drawing 2023-02-03 11.37.10.excalidraw|900]]


each cache entry is *tagged* using the address of the data word in main memory, requests for addresses can search the caches for matching tags, data is cached with its neighbors, based on the size of the cache line because of #spatial_locality. 
![[Pasted image 20230201185140.png]]
This entire line is loaded into L1 when needed. 
There are $2^S$ sets of cache lines. 
the top $32-S-O=T$ bits form the tag. 

#rem 
### Exclusive Cache
An **eviction** from L1d pushes the cache line down into L2, then L3 then to main memory. 
Each eviction is progressively more expensive.
Loading is faster as it only loads into one. 

### Inclusive Cache
Each cache line in L1 is present in L2, so eviction is faster, however, this wastes memory


![[Pasted image 20230202224749.png]]


# Cache #Coherency
---

### SMP
Symmetric multi processor systems 
	all processors see the same memory content at all times. 
	**cache concurrency** 
	![[Drawing 2023-02-08 11.42.43.excalidraw]]must be protected  
	when a **write access** is detected, the cache line is marked **invalid**, access in the future will need to reload it
	read is fine
	dirty cache is validated by the **snoopy bus**, forcing a reload```
	• A dirty cache line is not present in any other processor’s cache. 
	• Clean copies of the same cache line can reside in arbitrarily many caches```



## Snooping protocols
works when all memory access are performed via a shared medium that is seen by all cores, like a central bus. All memory access go through the central bus, then all accesss can be observed by the cache controller of each processor. 
	When it writes, it updates, copying the new value of from the bus into the cache. also called an *update based protocol* 
	*invalidation based protocols* are those where cache blocks corresponding to memory blocks are invalidated, then the next read must perform a read from main memory. 


### MSI / MESI (write back invalidation)
modified: 
	cache block contains the current value of the memory block and that all other copies of this memory block in other caches are invalid
shared:
	the cache block hasn't been updated in this cache, main memory and other caches 
invalid:
	the cache block doesn't contain the most recent value in the memory block
when modified, make others invalid, mark self modfied

Bus read:
	read from processor to memory block currently not stored in a cache of this processor, from another processor
Bus read Exclusive:
	bus read or not in M state, requests copy that it intends to modify, gets the most recent value, everything else marked invalid
Write Block:
	cache controller write cache block thats modified to main memory, main memory will be updated 






# Write behavior
### write through
every write to cache also write through to main memory, main always has the most recent, can be slow due to writing to main sometimes store stuff in write buffer, freed when write completes
## write back
the cache line is marked as dirty, then only when the cache line is dropped from the cache (because it ran out of space) then will it instruct the processor to write the data back at the time instead of discarding the content. Better performing. Must be assured that data is the same in all processors.

#### Policies
used for special regions of the address space not backed by real RAM. 
- **write combining** - used when transfer is expensive, combines multiple write accesses, only when ast word is written will it write to the device. 
- **uncacheable** - memory location not backed by RAM. for memory mapped address ranges. cannot cache these as they would slow down input. 


#### Dirty Cache
A cache line which has been written to and which has not been written back to main memory is said to be “dirty”. Once it is written the ***dirty*** flag is cleared. It's the most recent data.

## MESI
Mesi saves two bus transactions, when trying to read I state, MSI performs a read and a bus write to get to a shared state, then performs a BusRdEx attempting to invalidate all other caches, however, there are no other caches with that data in it. 

Modified: local processor modified the cache, the only copy in any cache. 
Exclusive: not modified, but not known to be loaded into any other processor's cache, a local read will create this state
Shared: cache line not modified and might exist in one or more caches 
Invalid: the line is unused or the data is now invalid. ![[Pasted image 20230214183908.png]]
Start invalid, loaded for writing then becomes modified. If for reading, if another processor has it than it's shared, else exclusive. If another processor wants to read from a modified line, the first processor sends the content of its cache to the second, the changing the state to shared, if it's write then it changes the state to invalid. 
This protocol must be distributed and have replies from all the processors. The longest reply time can determine speed of coherency protocol. FSB is shared and bandwidth is limited. 

### RFO Request For Ownership
1. when a thread is migrated from one processor to another, all cache lines have to be moved over to the new processor. 
2. cache line is needed in two different processors. 

- happens when another processor wants to write to a cache line that is marked as M, it marks itself I, then gives the contents to the other processor.
- When a processor is in the shared state, and it wants to write to it, it broadcasts RFO, invalidating everything else, intercepting all attempted reads, then writing the new data to main. Thus shared states are imprecise.

Bandwidth is the limiting factor, as adding threads means adding more need for concurrency, 

Sequential read access: the shared bus from the processor to memory controller and bus from the memory controller to the memory modules. 

In write through, the limiting factor is the larger, slower cache (L2)


### Critical Word load
- words go one by one, the cache line is multiple words
- it sends the critical word before the other words that come with the cache line
- hardware implemented

Bottlenecks
memory is transferred from the main memory in blocks that are generally smaller than the cache line size 

### 

# Consistency
---
### Sequential Consistency
- perceive the reads and writes in the same order globally by all processors, dictated by the program order, individual to processor
- does not guarantee things happen in order
- **atomic operation** - the effect of each memory operation must become globally visible to all processors before the next memory operation of any processor is started. 
### Sync/weak Consistency
- before a synchronization point, anything can happen. 
- after the synchronization point, every processor reads the same thing
- a collective, doesn't necessarily have to be blocking or synchronous, waits until it's in sync before using the variable
- OpenMP
	- barrier/ fence
	- [[SMPD]]
### Release Consistency
- break the sync operation into
	- acquire - tells the data store we are about to enter the [[Critical section]]
	- release - critical section is being exited

# Associativity

### **fully associative cache** 
![[Drawing 2023-02-06 11.03.41.excalidraw]]
processor compares tags of each and every cache line with the tag for the requested address the $S$ part, old used on small caches (few dozen), complicated transistor circuit. Fast. 
![[Pasted image 20230202225427.png]]
### **direct mapped cache**
![[Drawing 2023-02-03 11.46.50.excalidraw]]
match each tag with *one* tag entry, fast and east to implement. The number of multiplexers can become complex. Works well if addresses are evenly distributed with respect to the bits used for the direct mapping. Inexpensive, easy to implement
![[Pasted image 20230202225417.png]]
### **set-associative cache**
![[Drawing 2023-02-06 11.15.44.excalidraw]]
tag and data divided into sets. similar to direct mapped. There's a small number of value cached for the same set value. Tags are compared in parallel like direct mapped. 
peak size of working set is 5.6 million
![[Pasted image 20230202225720.png]]

$$cache\ size = cache \ line\ size \times associativity \times number\ of\ sets$$
Addresses are mapped into the cache using:
$O = log_{2}\ cache\ line\ size$
$S = log_{2}\ number\ of \ sets$

#hyperthreading L1 cache is shared, multicore processors use shared L2. 
Associativity of caches grow with increasing numbers of cores. 

because of #spatial_locality , the CPU **prefetches** the next cache line. this can decrease time from 200+ cycles to around 9. prefetching cannot cross page boundaries

[[TLB]] cache can miss

#### Misses
cold start misses - when first start up, nothing in the cache is usable
capacity misses - when fully associative cache is full, replace to make room
	prevent by using LRU, least recently used
![[Drawing 2023-02-06 11.30.10.excalidraw]]



### How to make cache more efficient
- make sure data is sequential so that prefetching works
- keep data small so it doesn't go beyond page breaks, using cache lines instead of pages with TLB, if they are large TLB overflows and new pages are called every time. 
- forced prefetching is possible when a line uses data that the program is already going to get to, this however, runs out of L2 cache faster, and has to use memory, becoming slower in the long run. 
- randomizing can cause automated prefetching to overfetch and run out of room, the miss ratio is higher

```
C volatile
for reading from devices, memory isn't updated in write back systems, 
bypass the cache so it always read from memory

ldwio
does not cache

memory data will always be the most recent one
```
