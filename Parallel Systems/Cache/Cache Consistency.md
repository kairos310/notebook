### Sequential Consistency
- perceive the reads and writes in the same order globally by all processors, dictated by the program order, individual to processor
- does not guarantee things happen in order
- **atomic operation** - the effect of each memory operation must become globally visible to all processors before the next memory operation of any processor is started. 
- each processor must follow the sequential 


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
