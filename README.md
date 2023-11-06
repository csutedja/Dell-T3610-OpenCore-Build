# Dell Precision T3610 OpenCore Build
Guide on how to get macOS versions 10.15/11 running on a Dell Precision T3610 workstation. 


### Important information

⚠ **WARNING!** You cannot and will never be to run macOS Monterey or higher (12.0+) on this machine. This is because due to its age, the T3610's motherboard lacks the ability to perform a TSC reset at boot-time, <a href="https://github.com/acidanthera/CpuTscSync#cputscsync">and the CPU also lacks a certain register needed to do this on Monterey</a>. However, Catalina and Big Sur run great on this system.


⚠ **Don't** use the EFI folder included, it will not boot. You should use it as a guide for which files should go in which folders.

⚠ Make sure you have the **latest** BIOS revision installed before proceeding. Windows is required to update. <a href="https://www.dell.com/support/home/en-us/drivers/driversdetails?driverid=4d5hg">(BIOS Revision A19 directly from Dell)</a>

##### What works:
- GPU acceleration (I use a PowerColor AMD RX 550 which is not natively supported, I will provide a .ssdt file to get that working if you have that card)
- App Store
- iMessage/FaceTime (requires generated iMacPro1,1 SMBIOS)
- Ethernet
- WiFi (will vary depending on your dongle/wireless card)
- Sound (on-board audio/GPU out)
- Bluetooth (w/ USB dongle)
- DRM (AppleTV/Netflix/etc.)
- 4k display
- Big Sur Security Patches/Updates
- CPU Power Management with <a href="https://dortania.github.io/OpenCore-Post-Install/universal/pm.html#sandy-and-ivy-bridge-power-management">ssdtPRGen</a>
- USB 2.0 ports, mapped with <a href="https://github.com/USBToolBox/tool">USBToolBox</a> 

##### What doesn't work:
- macOS versions past Big Sur (read above)
- Built-in USB 3.0 (use an expansion card if needed)
- Sleep (supposedly works with modded BIOS but I'm not going to try that) 
- Sidecar (not tested)

##### Make sure you have these BIOS settings (taken from cstrouse's OpenCore guide):
- Reset to optimized defaults
- Secure Boot disabled
- Enable VT for Direct I/O disabled (Virtualization Support can be enabled if you need it for Docker, etc)
- Disks set to AHCI mode (default is RAID)
- Fast Boot set to 'thorough'
- CPU XD support enabled
- TPM disabled
- Legacy ROM disabled (leave on if you have a Quadro, AMD users disable)
- USB 3.0 can be kept on for Windows/Linux/etc. but keep in mind it will not work in macOS


# Installation

Start here, following guides for _Ivy Bridge-E_. 

https://dortania.github.io/OpenCore-Install-Guide/

https://dortania.github.io/OpenCore-Install-Guide/config-HEDT/ivy-bridge-e.html

_Note 1: when generating an SMBIOS use **iMacPro1,1**_
_Note 2: AppleCpuPmCfgLock and AppleXcpmCfgLock **must** be enabled for your system to boot, as you cannot disable it on the T3610 easily___
_Note 3: make sure **XhciPortLimit** is disabled, your ports should be mapped before installing_


  
