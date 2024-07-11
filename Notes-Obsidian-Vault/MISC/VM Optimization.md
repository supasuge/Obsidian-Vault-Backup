## Table of Contents

  - [Enabling PAE/NX In Virtual box](#Enabling\PAE/NX\In\Virtual\box)


`NX` bit:
- `NX` bit is used in CPUs to segregate areas of memory for use by either storage of processor instructions (code) or for storage of data
- Marks certain areas of memory as non-executable, meaning that any code residing in these areas cannot be run. This helps in preventing exploits such as buffer overflows for example.



## Enabling PAE/NX In Virtual box
- Allowing the Vm to utilize these processor features.
**Pros:**
- Allows 32-bit VMs to use more than 4GB of RAM. Enhancing performance.
- `NX` increases the security of the BM
**Considerations**:
- **Compatibility**: Ensure that the guest OS supports PAE/NX. Most modern operating systems do, but enabling it on an unsupported OS can lead to issues.
- **Performance Impact**: Enabling PAE can have a slight performance impact, though it's usually negligible. The security benefits of NX generally outweigh any minor performance considerations.

