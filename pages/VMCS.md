- ((65217199-f8de-4d20-9a60-4ab081661a53))
- **Virtual machine control structure (VMCS)**: is a data structure in memory that exists exactly once per VM (or more precisely one per each VCPU [Virtual CPU]), while the VMM manages it. With every change of the execution context between different VMs, the VMCS is restored for the current VM, defining the state of the VM’s virtual processor and VMM control Guest software using VMCS.
  id:: 652171a8-921e-455e-b67a-7a728cf92f74
- The VMCS consists of six logical groups:
	- Guest-state area: Processor state saved into the guest state area on VM exits and loaded on VM entries.
	  id:: 652171de-ce4c-4eb1-a187-f68b509847dc
	- Host-state area: Processor state loaded from the host state area on VM exits.
	- VM-execution control fields: Fields controlling processor operation in VMX non-root operation.
	- VM-exit control fields: Fields that control VM exits.
	- VM-entry control fields: Fields that control VM entries.
	- VM-exit information fields: Read-only fields to receive information on VM exits describing the cause and the nature of the VM exit.