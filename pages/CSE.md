## Inode-FS
	- 5 Layers
		- Block Layer:
			- 4KB Size per block (maybe but adjustable)
			- Contains bitmap to track the usability of the block
			  id:: 654f666b-a243-49f6-8077-f1324f3092b4
		- File Layer:
			- **Representative**: Inode
			- Resides in the block too
			- Holds a array organized inode table to map the inode number to the block number that stores the inode
			- ```
			  Function
			  ```
		-