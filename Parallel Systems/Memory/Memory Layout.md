
### PC and smaller servers (First commodity hardware)
![[Pasted image 20230124214245.png]]
- all data communication from one CPU to another must travel through the same bus used to communicate with the Northbridge
- All communication with RAM goes through the northbridge
- RAM has a single port
- Communication between CPU and device routed through the northbridge

---
## Northbridge
contains a memory controller depending on type of RAM
to communicate with everything else it talks to the southbridge

## Southbridge
referred to as the I/O bridge
handles communitation with other devices with different buses.

---
### Northbridge with External Controllers
![[Pasted image 20230124214315.png]]
- more than one memory bus exists
- total available bandwidth increases
- supports more memory
- memory is concurrent

### Integrated Memory Controller
![[Pasted image 20230124214300.png]]
- memory of the system needs to be accessible to all processors
- memory is not uniform
- [[Distributed Memory Architecture]]
- **NUMA** (Non Uniform Memory Architecture)
	- memory attached to a different processor, memory retrieval is slowed depending on how many levels of interconnects away it is #rem
