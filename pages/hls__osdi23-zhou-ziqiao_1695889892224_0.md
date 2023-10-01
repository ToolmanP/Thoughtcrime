file:: [osdi23-zhou-ziqiao_1695889892224_0.pdf](../assets/osdi23-zhou-ziqiao_1695889892224_0.pdf)
file-path:: ../assets/osdi23-zhou-ziqiao_1695889892224_0.pdf

- the hypervisor retains control of resource management and observes associated guest actions including nested page table faults and CPU scheduling, and thus confidential VMs remain vulnerable to an ever-changing variety of hypervisor-level side channel attacks. 
  ls-type:: annotation
  hl-page:: 2
  hl-color:: red
  id:: 65153b00-e54b-495b-9577-b77d66cc3a6d
- To ensure isolation without the complexity of virtualization, we propose simple hardware extensions that restrict guests to a static slice of a machine’s cores, memory and virtual I/O devices, and delegate resource allocation to a dedicated management slice.
  ls-type:: annotation
  hl-page:: 2
  hl-color:: blue
  id:: 65153c32-5f1a-4006-b2a1-b7265791d203
- We observe that, although cloud guests may benefit from a VM-level execution abstraction, cloud providers do not exploit the full complexity enabled by hypervisor-based virtual machines for IaaS workloads. For example, although hypervisors support time-slicing VMs on shared cores, VMs offered by major public cloud providers including Amazon [11] and Azure [ 81] are sized at core granularity and scheduled on distinct physical cores [ 7 , 76, 77 ]. Likewise, the memory allocated to guest VMs is static; techniques such as memory ballooning [ 118 ] or transparent page sharing [ 118 , 124 ] are avoided. Cloud providers are also moving to reduce the overhead of I/O virtualization by offloading I/O processing to dedicated hardware [ 5 , 8 , 36 ]. To ensure that resources sold match those available, cloud providers limit oversubscription to only their own (first-party) VMs [ 28 ] or disable it entirely [ 7]. Overall, although today’s cloud runs virtual machines, leading public cloud providers do so using an effectively static allocation of cores and memory. The hypervisor is relied upon for isolation, but it does so merely by partitioning platform resources.
  ls-type:: annotation
  hl-page:: 3
  hl-color:: blue
  id:: 651545fb-4787-45d4-828a-543ffa0b1bc1
  hl-stamp:: 1695892989474
- While core slicing eliminates intra-core leakage, it does not prevent cross-core side channels such as CrossTalk [ 91 ] which will have to be addressed by other means. 
  ls-type:: annotation
  hl-page:: 3
  hl-color:: red
  id:: 651546da-6c0e-44e8-a08d-9b4f69fe0794
- it stops execution, resets the entire core to a well-defined architectural state(resetting locked registers), and causes the core to begin execution at a fixed address. 
  ls-type:: annotation
  hl-page:: 7
  hl-color:: green
  id:: 6516cbff-567d-426a-be9c-0de63ca33486
- only the slicevisor running in the control slice’s privileged mode can initiate such a reset
  ls-type:: annotation
  hl-page:: 7
  hl-color:: blue
  id:: 6516cc2b-a7b6-43bf-8418-f870980d7f3d
- he address of the jump target that receives control after reset remains inaccessible to any user slice.
  ls-type:: annotation
  hl-page:: 7
  hl-color:: yellow
  id:: 6516cc32-b4db-4817-88cd-42ce2ecec78d
- The only other code that must be trusted by guests is the sliceloader which is the first thing to run after a core is reset by the slicevisor. It is responsible for configuring and locking the per-core access filters, and then coordinating with other loaders of the slice to securely boot the guest OS.
  ls-type:: annotation
  hl-page:: 8
  hl-color:: green
  id:: 6516d06e-043a-4b40-9575-9b066bba9ec4
- Range checks are trivially parallelizable and can be applied before installing a page translation in the TLB, thus having negligible runtime overhead. 
  ls-type:: annotation
  hl-page:: 7
  hl-color:: red
  id:: 6516d23c-6c07-4767-947b-320453fad47d
- Rather than reprogramming the IOMMU at runtime to restrict DMA, recent work found that it is more efficient to simply allocate all I/O buffers from a dedicated pool of physical memory that remains mapped in the IOMMU [74]; this is naturally supported by core slicing, and avoids the need for runtime interaction with slicevisor to reprogram a slice’s IOMMU translations
  ls-type:: annotation
  hl-page:: 8
  hl-color:: green
  id:: 6516d2d6-05bf-42a2-8a74-5c5191a8393e
- Because confidential VMs inherit from SGX the key design feature of a privileged attacker who controls resources and shares processor time with the TEE, we expect them to remain vulnerable to many forms of side-channel attacks similar to those that devastated SGX
  ls-type:: annotation
  hl-page:: 4
  hl-color:: blue
  id:: 6517023b-afbb-4f3c-9811-afecbbd0a136
- Cloud vendors have deployed dedicated hardware “cards” that replace software I/O virtualization stacks, exposing virtual devices to guests directly via SR-IOV. F
  ls-type:: annotation
  hl-page:: 5
  hl-color:: green
  id:: 651702c6-5ed6-4745-8eec-7f3f5564c8eb
- When booting a guest, the sliceloader copies itself to private slice memory before setting PMPs, because doing so makes the main copy inaccessible.
  ls-type:: annotation
  hl-page:: 10
  hl-color:: purple
  id:: 651703e5-aa85-4ea8-93ad-67c40217e69c
- To allow Linux to boot in a slice, we enlightened the OpenSBI bootloader [94 ] to run as untrusted code inside a newly-created slice. It constructs a devicetree describing the slice configuration before booting Linux.
  ls-type:: annotation
  hl-page:: 10
  hl-color:: purple
  id:: 651703f4-80f1-467c-b41a-2ad88564b6d7