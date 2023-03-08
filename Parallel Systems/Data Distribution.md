
## Blockwise
length n cuts the array into p blocks with n/p consecutive elements each.
When n is not a multiple of p, the last block contains less than n/p elements. 
## Cyclic
one-dimensional array assigns the array elements in a round robin way to the processors so that array element vi is assigned to processor $P_{(i−1)} mod(p) +1, i = 1,..., n$
## Block Cyclic
Consecutive array elements are structured into blocks of size b, where b  n/p in most cases. When n is not a multiple of b, the last block contains less than b elements. 

![[Drawing 2023-02-22 11.14.42.excalidraw|600]]
![[Pasted image 20230222111736.png]]

## Checkerboard
#### blockwise cyclic
the array is decomposed into p1 · p2 blocks of elements where the row dimension (first index) is divided into p1 blocks and the column dimension (second index) is divided into p2 blocks.
#### block-cyclic
round-robin in both dimensions
An alternative way to describe the cyclic checkerboard distribution is to build blocks of size p1× p2 and to map element (i, j) of each block to the processor at position (i, j)in the mesh.
![[Pasted image 20230228224232.png]]
## Parameterized data distribution
