# Dell Precision T3610 OpenCore Build
Guide on how to get up to macOS Big Sur almost fully working on a Dell Precision T3610 workstation. 

Before you proceed:


⚠ **WARNING!** You cannot and will never be to run macOS Monterey or higher (12.0+) on this machine. This is because due to its age, the T3610's motherboard lacks the ability to perform a TSC reset at boot-time, <a href="https://github.com/acidanthera/CpuTscSync#cputscsync">and the CPU also lacks a certain register needed to do this on Monterey</a>. However, Catalina and Big Sur run great on this system.


⚠ This is **not** a pre-made EFI folder. The EFI folder provided in this repository is just an example of where to put your files. This is because kexts/OpenCore update all the time, and I don't want to provide outdated files.  

⚠ Make sure you have the latest BIOS revision installed before proceeding. Windows is required to update. <a href="https://www.dell.com/support/home/en-us/drivers/driversdetails?driverid=4d5hg">(BIOS Revision A19 directly from Dell)</a>

What works:
- GPU acceleration (I use a PowerColor AMD RX 550 which is not natively supported, I will provide a .ssdt file to get that working if you have that card)
- App Store
- iMessage/FaceTime (requires generated iMacPro1,1 SMBIOS)
- Ethernet
- WiFi (will vary depending on your dongle/wireless card)
- Sound (on-board speaker/)
- Bluetooth (through a USB dongle)
- DRM (AppleTV/Netflix/etc.)
- 4k display
- Software Updates
- CPU Power Management with <a href="https://dortania.github.io/OpenCore-Post-Install/universal/pm.html#sandy-and-ivy-bridge-power-management">ssdtPRGen</a>

What doesn't work:
- macOS versions past Big Sur (read above)
- Built-in USB 3.0 (use an expansion card if needed)
- Sleep (supposedly works with modded BIOS but I find that too risky personally) 
- Sidecar (not tested)

BIOS settings (taken from cstrouse's guide):
- Reset to optimized defaults
- Secure Boot disabled
- Enable VT for Direct I/O disabled (Virtualization Support can be enabled if you need it for Docker, etc)
- Disks set to AHCI mode (default is RAID)
- Fast Boot set to 'thorough'
- CPU XD support enabled
- TPM disabled
- Legacy ROM disabled (required for Quadro but not for Radeon)

With that out of the way:

Start here, following guides for _Ivy Bridge-E_. 

https://dortania.github.io/OpenCore-Install-Guide/

https://dortania.github.io/OpenCore-Install-Guide/config-HEDT/ivy-bridge-e.html


  
