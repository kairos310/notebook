Translation lookaside buffer
stores recent translation of virtual memory to physical addresses for faster retrieval. 
stores only mapping unlike cache that stores data
small and extremely fast, some have multi levels. 

processor core global resource. 
all threads and processes executed on the processor core use the same TLB.
##### Two solutions
- *Flushing* 
	- The TLB is flushed whenever the page table tree is changed. 
	- whenever context switch performed
	- requires [[kernel]] code
	- expensive
	- replace as many TLB entries as pages are touched
	- optimization
		- individually invalidate TLB entries, if kernel code and data falls into specify address range, only the pages falling into their address range are evicted .
- *Extending*
	- The tags for the TLB entries are extended to additionally and uniquely identify the page table tree they refer to
	- If the memory use (and hence TLB entry use) of each of the runnable processes is limited, there is a good chance the most recently used TLB entries for a process are still in the TLB
	- Without tags, one or two TLB flushes are performed. With tags the calling address space’s cached translations are preserved and, since the kernel and VMM address space do not often change TLB entries at all
	- When switching between two threads of the same process no TLB flush is necessary at all. Without extended TLB tags the entry into the kernel destroys the first thread’s TLB entries, though.


## Performance
---
#### Page sizes
1. wasted memory 
	1. because memory regions need to be *contiguous* and others need to have *alignment*, not practical to *increase* size of pages. 
2. Number of levels of page tree reduced

## Virtualization
---
- In the case of Xen virtualization, the Xen VMM controls access to physical memory, but other hardware controls are handled by the privileged Dom0 domain. 
	- The memory handling in the Dom0 and DomU kernels is implemented through the creation of page table trees, and the VMM controls access to the administrative information of these data structures.
	- The guest domains construct their page table trees for each process, and the VMM is invoked whenever the guest OS modifies its page tables to update the administrative information in the guest domain. This access control is used in both para and hardware virtualization.
- Using shadow page tables for memory handling in virtualization is expensive because each modification of the page table tree requires an invocation of the VMM, and changes from the guest OS to the VMM and back are also expensive. 
- Avoid creating shadow page tables
	- Processors like Intel and AMD have implemented technologies like EPTs and NPTs to reduce memory consumption and improve memory handling speed at almost the speed of the no-virtualization case. The additional address translation steps are stored in the TLB.
- The article also discusses the problem inherent in VMM-based virtualization, which is having two layers of memory handling, making optimal memory handling difficult. 
	- However, the KVM Linux kernel extensions solve this problem by using a normal Linux kernel to manage memory, which reduces the work and the number of bugs. 
- Finally, the article reminds programmers that the cost of cache misses is even higher with virtualization, so optimizing to reduce this work will pay off even more in virtualized environments.