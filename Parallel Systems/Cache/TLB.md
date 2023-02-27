# Translation lookaside buffer

stores recent translation of virtual memory to physical addresses for faster retrieval 
stores only mapping unlike cache that stores data
small and extremely fast, some have multi levels. 

processor core global resource 
CPU cannot blindly reuse the cached entries if the page table is changed, 
all threads and processes executed on the processor core use the same TLB.
##### Two solutions
- *Flushing* 
	- The TLB is flushed whenever the page table tree is changed. 
	- whenever context switch performed, has to resume execution of different process or [[Kernel]]
	- expensive
	- replace as many TLB entries as pages are touched
	- optimization
		- individually invalidate TLB entries, if kernel code and data falls into specify address range, only the pages falling into their address range are evicted.
- *Extending*
	- The tags for the TLB entries are extended to additionally and uniquely identify the page table tree they refer to
	- If the memory use (and hence TLB entry use) of each of the runnable processes is limited, there is a good chance the most recently used TLB entries for a process are still in the TLB
		- Without tags, one or two TLB flushes are performed. With tags the calling address space’s cached translations are preserved and, since the *kernel and VMM address space do not often change* TLB entries at all
		- When *switching between two threads* of the same process no *TLB flush is necessary* at all. Without extended TLB tags the entry into the kernel destroys the first thread’s TLB entries, though.
	- allows the OS to avoid flushing the guest’s TLB entries every time the VMM is entered


## Performance
---
#### Page sizes
1. wasted memory 
	1. because memory regions need to be *contiguous* and others need to have *alignment*, not practical to *increase* size of pages. 
2. Number of levels of page tree reduced
