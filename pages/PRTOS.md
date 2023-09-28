- PRTOS stands for partitioned real-time operating system which is another kind of RTOS with Type-1 system partitioning on supervisor mode. Dedicated to reservation on system resources for each partition, the hardware level system segeragation provides another layer of complete virtualization.
- ## RTOS Overview
	- ### Shortcomings
	- **RTOS should support a large variety of devices exponentializing the system's complexity since the driver holds a considerable amount of operating code.**
	  logseq.order-list-type:: number
	- **RTOS faces  rapid resource racing since every task is placed under the kernel mode.**
	  logseq.order-list-type:: number
	- **RTOS still uses MMU and MPU to protect the address space which leads to significant runtime overhead.**
	  logseq.order-list-type:: number
- ## Concepts
	- ### Time Partitioning
		- Time pieces allocation
	- ### Resource Partitioning
		- Memory address space protection
	- ### Design pattern
		- Resources are separated at the bare-metal level, the OS kernel running on it can not perceive what's happening.
		- Panic is restrained in each partition and hypervisor should be able to intercept and handle them and contain them outside of bare-metal environment.
		- Resource is a lease and recycled after a lease.
		- Inter-partition Communication is only contained and reduced to a minimal levels.
-
- logseq.order-list-type:: number
-
- ## PRTOS Goals
- Resources separation on bare metal levels instead of OS-handling.
  logseq.order-list-type:: number
- Bare-metal level hypervisors
  logseq.order-list-type:: number
- Reduce the extent of coupling by building several different controlling modules. (maybe a system-level micro service of hypervisors)
  logseq.order-list-type:: number
- Produce a separation kernel based a hardware driven hypervisor
  logseq.order-list-type:: number
-