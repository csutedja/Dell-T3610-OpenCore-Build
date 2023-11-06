# Dell Precision T3610 OpenCore Build
In-depth guide on how to get macOS versions 10.15/11 running on a Dell Precision T3610 workstation. 


### Important information

⚠ **WARNING!** You cannot run macOS Monterey or higher (12.0+) on this machine. This is because due to its age, the T3610's motherboard lacks the ability to perform a TSC reset at boot-time, <a href="https://github.com/acidanthera/CpuTscSync#cputscsync">and the CPU also lacks a certain register needed to do this on Monterey</a>. However, Catalina and Big Sur run great on this system.


⚠ **Do not** use the EFI folder included, it will not boot. You should use it as a guide for which files should go in which folders.

⚠ Make sure you have the latest BIOS revision installed before proceeding. Windows is required to update. <a href="https://www.dell.com/support/home/en-us/drivers/driversdetails?driverid=4d5hg">(BIOS Revision A19 directly from Dell)</a>

⚠ This guide is for macOS Catalina and Big Sur. However, macOS High Sierra/Mojave probably work (anything lower was never supported by the iMac Pro), I just haven't tested it. 


##### What works:
- GPU acceleration (I use a PowerColor RX 550 with no official Mac support, read in the Extras section for a fix.)
- App Store
- iMessage/FaceTime (requires generated iMacPro1,1 SMBIOS)
- Ethernet
- WiFi (depends on adapter/card, read more <a href="https://dortania.github.io/Wireless-Buyers-Guide/">here</a>)
- Sound (on-board audio/GPU out)
- Bluetooth (I linked a compatible adapter below)
- DRM (AppleTV/Netflix/etc.)
- 4k display
- Big Sur Security Patches/Updates
- CPU Power Management with <a href="https://dortania.github.io/OpenCore-Post-Install/universal/pm.html#sandy-and-ivy-bridge-power-management">ssdtPRGen</a>
- USB 2.0 ports, mapped with <a href="https://github.com/USBToolBox/tool">USBToolBox</a> 

##### What doesn't work:
- macOS versions past Big Sur (read above)
- Built-in USB 3.0 (supposedly the Inateck PCIe card works if you need those ports)
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

- Note 1: when generating an SMBIOS use **iMacPro1,1** model
-  _Note 2: AppleCpuPmCfgLock **and** AppleXcpmCfgLock **must** be enabled for your system to boot, as you cannot disable it on the T3610 easily_
-  Note 3: make sure **XhciPortLimit** is disabled, your ports should be mapped before installing anyways.
-  Note 4: as stated in the guide, if you are running Catalina or older make sure the Apple Secure Boot setting is set to **Disabled*

# Post-installation
Assuming you carefully followed the above guides and instructions, macOS should boot properly. However, there are a few specific post install tips that I highly recommend that I will link below:

- https://dortania.github.io/OpenCore-Post-Install/#universal (all of these guides are highly recommended for a good experience but the sleep fix will probably not work. you can try it though)
- https://dortania.github.io/OpenCore-Post-Install/cosmetic/verbose.html (also highly recommended, drastically cleans up the boot process)
- https://dortania.github.io/OpenCore-Post-Install/cosmetic/gui.html#setting-up-opencore-s-gui (not needed, just provides a nice boot menu)

# Extras 
The following tips are not needed and are more based on my own experiences and hardware, so if you run in to these situations look at the following:

##### Wi-Fi Adapter (EP-N8508GS)
Before I switched to using Ethernet which works out-of-the-box on macOS with the correct kext, I used to use an EDUP EP-N8508GS USB adapter, which is based on a Realtek chipset not natively supported in macOS. The best fix I have found is <a href="https://github.com/chris1111/Wireless-USB-Big-Sur-Adapter">this tool</a> by chris1111. (you will need to disable SIP temporarily as it installs system extensions, if you are uncomfortable with disabling SIP consider using Ethernet or a <a href="https://dortania.github.io/Wireless-Buyers-Guide/types-of-wireless-card/pcie.html">natively supported PCIe Wi-Fi card.</a>) You can re-enable SIP after installation however. I believe _RtWlanU.kext_ and _RtWlanU1827.kext_ are also required in /EFI/OC/Kexts and your config.plist. I have linked those kexts in the Extras folder in my repository.

##### Bluetoooth
The T3610 does not ship with any kind of bluetooth. However, I use <a href="https://www.amazon.com/IOGEAR-Bluetooth-Multi-Language-Version-GBU521W6/dp/B007ZT2AXE?th=1" rel="nofollow noreferrer">this inexpensive USB bluetooth adapter</a> (not a refferal link) which works in macOS with no additional setup. This adapter also works great in Windows and Linux. AirDrop/Handoff/AirPods switching between devices/whatever Apple feature work properly with this adapter. For less clutter you can put the adapter in the motherboard's internal USB2 port. (provided you mapped the port properly with USBToolBox)








 


  
