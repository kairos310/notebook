managed by the [[MMU]]
OS fills out page table data structures

## page table
one entry for every word in virtual address - $2^{30}$, **frame** 
## translation 
divide memory into pages, chunks. each entry can map a few kB of data. 
have to move a whole page at a time, so the larger the page the slower. 
the offset in the physical and virtual stays the same, this doesn't get translated. 
page number gets translated through the page table (lookup)

![[Pasted image 20230225132431.png]]
The complete physical address of the memory cell is determined by combining the page address from the page directory with the low bits from the virtual address
**page address**:
| page address | low bits from physical address | access permission |
| ------------ | ------------------------------ | ----------------- |



![[Pasted image 20230225132845.png]]

1. processor determines address of the highest level directory
2. takes the index of virtual address, picks the appropriate entry
3. this entry is the address of the next directory
4. 🔁 repeats until the last process 

Each process has its own page table tree, not all programs use all 4 levels.
the complete computation of addresses of physical page is cached into the [[TLB]]



## Virtualization
---
- In the case of Xen virtualization, the Xen VMM controls access to physical memory, but other hardware controls are handled by the privileged Dom0 domain. 
 ![[Pasted image 20230226220223.png]]
	- The memory handling in the Dom0 and DomU kernels is implemented through the creation of page table trees, and the VMM controls access to the administrative information of these data structures.
	- The guest domains construct their page table trees for each process, and the VMM is invoked whenever the guest OS modifies its page tables to update the administrative information in the guest domain. This access control is used in both para and hardware virtualization.
### KVM 
run multiple local environments called guests.
- Using shadow page tables for memory handling in virtualization is expensive because each modification of the page table tree requires an invocation of the VMM, and changes from the guest OS to the VMM and back are also expensive. 
- Avoid creating shadow page tables
	- Processors like Intel and AMD have implemented technologies like EPTs and NPTs to reduce memory consumption and improve memory handling speed at almost the speed of the no-virtualization case. The additional address translation steps are stored in the TLB.
- The article also discusses the problem inherent in VMM-based virtualization, which is having two layers of memory handling, making optimal memory handling difficult. 
	- However, the KVM Linux kernel extensions solve this problem by using a normal Linux kernel to manage memory, which reduces the work and the number of bugs. 
- Finally, the article reminds programmers that the cost of cache misses is even higher with virtualization, so optimizing to reduce this work will pay off even more in virtualized environments.