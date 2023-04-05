
# Write behavior
### write through
every write to cache also write through to main memory, main always has the most recent, can be slow due to writing to main sometimes store stuff in write buffer, freed when write completes
### write back
the cache line is marked as dirty, then only when the cache line is dropped from the cache (because it ran out of space) then will it instruct the processor to write the data back at the time instead of discarding the content. Better performing. Must be assured that data is the same in all processors.


### Policies
used for special regions of the address space not backed by real RAM. 

#write_combining
<div class ="def" value="#write_combining">
<b>Write Combining </b> is used when transfer is expensive, combines multiple write accesses into a singular cache line, only when last word is written will it write to the device.
</div>

- **uncacheable** - memory location not backed by RAM. for memory mapped address ranges. cannot cache these as they would slow down input. 


### Dirty Cache
A cache line which has been written to and which has not been written back to main memory is said to be “dirty”. Once it is written the ***dirty*** flag is cleared. It's the most recent data.
