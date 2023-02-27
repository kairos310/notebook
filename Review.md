# Quizzes
---
In the first commodity hardware system, the role of the Northbridge and Southbridge. 
- Northbridge connects the CPU and RAM through the FSB. [[Memory Layout]]
- Southbridge connects the Northbridge with external i/o
	- USB, PCIE, SATA, 
- limited by the FSB, singular RAM port. 


#### *Alternative solutions to the first commodity hardware system.* 
External controllers
Integrated memory controllers

advantage: 
	CPU to memory connected no longer limited by FSB bandwidth
disadvantage:
	**NUMA** (Non Uniform Memory Architecture)
	- memory attached to a different processor, memory retrieval is slowed depending on how many levels of interconnects away it is

##### *DRAM vs SRAM*
[[RAM]]
**SRAM**
1. more complex, 6 transistors
2. more expensive 
faster, not limited b discharge speeds, needs power line and not refresh

**Four problems with DRAM**
1. needs refreshing every 64 ms
	- can use up to 50% memory
2. information from cell needs to be amplified
3. reading cell discharges cell, operation is followed by feeding output back in
4. charging and draining is not instantaneous

---
##### Working Set
site of the memory reload for a program, the page

##### Exclusive cache
only one cache gets the data, when not used, data is evicted up the hierarchy 
[[cache]]
##### Inclusive Cache
