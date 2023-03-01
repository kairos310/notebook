### Thread Level parallelism
explicit programming to handle threads

## Multithreading
virtual processor, separate program counters
### interleaved multithreading
aka fine grained multithreading
memory latency is hidden because of switch to other task

### coarse gained multithreading
- is where switches only happens on stalls. 
- *time sliced multithreading* is a variant, switching based fixed time interval
- *switch on event multithreading* switch on a wait
	- *simultaneous multithreading* and *hyper threading* fall into this

### simultaneous multithreading
