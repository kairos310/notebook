managed by the [[MMU]]
OS fills out page table data structures

### page table
one entry for every word in virtual address - $2^{30}$
##### translation 
divide memory into pages, chunks. each entry can map a few kB of data. 
have to move a whole page at a time, so the larger the page the slower. 

the offset in the physical and virtual stays the same, this doesn't get translated. 
page number gets translated 
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
