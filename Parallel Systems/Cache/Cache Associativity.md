
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

## Tags
each cache entry is *tagged* using the address of the data word in main memory, requests for addresses can search the caches for matching tags, data is cached with its neighbors, based on the size of the cache line because of #spatial_locality. 


![[Pasted image 20230201185140.png]]
This entire line is loaded into L1 when needed. 
There are $2^S$ sets of cache lines. 
the top $32-S-O=T$ bits form the tag. 
