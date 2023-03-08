
# Interconnects
---
<div style="padding: 1em; background:#181818">links all all Compute Nodes together
*eg. ethernet cable, InfiniBand switch*</div>

### Static interconnect
all nodes connected to each other, all one hop away from each other

### Dynamic interconnect
indirect - some nodes aren't connected to each other and there are branches and **switches** and **links**
eg. InfiniBand

![[Drawing 2023-02-03 11.08.04.excalidraw]]
Fat tree networks



# Topology
[[Network Topology]]


# Routing
## MPI
message passing interface
negotiates send and receive commands
sort of like an API to pass information to one another
	openMPI
	MPICH
	MVAPICH


### PGAS
partitioned GAS
doesn't need the node to open receive command, get node can direrctly retrieve data
	openSHMEM
	UPC
	ARMCI

### FSB
front side bus

