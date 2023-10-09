- ![TNC_TAP_Information_Model_v1.00_r0.36-FINAL.pdf](../assets/TNC_TAP_Information_Model_v1.00_r0.36-FINAL_1696688912073_0.pdf)
- ## TPM
- > The [*Trusted Platform Module (TPM)*](https://learn.microsoft.com/en-us/windows/security/information-protection/tpm/trusted-platform-module-top-node) technology is designed to provide hardware-based, security-related functions. A TPM chip is a secure crypto-processor that is designed to carry out cryptographic operations. The chip includes multiple physical security mechanisms to make it tamper-resistant, and malicious software is unable to tamper with the security functions of the TPM.
- In short, TPM acts a trusted authority and holds a series of asymetric private keys to
	- Secure the intergrity and authenticity of the boot code.
	  logseq.order-list-type:: number
	- Avoid any malicious software from tampering the fundamental components.
	  logseq.order-list-type:: number
	- Prevent untrusted devices from interacting with the system.
	  logseq.order-list-type:: number
- ## TCG
- TCG is an attestation protocol, independent from the transmission, which provides a secure and reliable information for the verifier to inspect the integrity of the requested attester's components.