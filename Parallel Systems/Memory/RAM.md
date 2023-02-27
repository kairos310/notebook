#### Static RAM
![[Pasted image 20230124215748.png]]
- 6 transistors
- more complex
- stable as long as power is supplied
- no refresh cycles needed

#### Dynamic RAM
![[Pasted image 20230124215754.png]]
- simpler (one transistor and one capacitor)
	- tiny capacitors femto-farad
- cheaper
- keeps state in capacitor

**Four problems with DRAM**
1. needs refreshing every 64 ms
	- can use up to 50% memory
2. information from cell needs to be amplified
3. reading cell discharges cell, operation is followed by feeding output back in
4. charging and draining is not instantaneous

The number of address lines is directly responsible for the cost of the memory controller, mother boards. 

### DMA (Direct Memory Access)
solution to NUMA
uses FSB bandwidth
high DMA traffic can cause the CPU to stall
