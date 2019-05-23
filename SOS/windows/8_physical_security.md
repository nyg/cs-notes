# Physical security

* Full Disk encryption with Bitlocker.
* Secure boot protects the boot process by checking digital signatures before loading components.
* Cold boot attack: researchers shown in 2008 that cooling down the volatile memory increase retention time enough to recover secret key from it. Use a PIN to prevent the attack and hibernate instead of sleep.
* Direct Memory Access is a mechanism allowing a device to access the physical memory directly. Researchers created in 2008 a rogue Firewire device able to read and write memory. Enable Kernel DMA Protection.
