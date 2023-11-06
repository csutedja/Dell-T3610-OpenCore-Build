# Dell Precision T3610 OpenCore Build
In-depth guide on how to get macOS versions 10.15/11 running on a Dell Precision T3610 workstation. _(Last updated 11/06/2023)_


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
some rambly tips and tricks based on hardware/software complications I've experienced

##### Sleep
After trying for many months I have concluded that proper sleep/wake is just not possible on this computer. The system will shut the screen off, enter hibernation, and will be unable to wake back up. My partial fix is to make a small script with the _caffeinate_ command that launches at login, which will prevent your system from hibernating, however this could lead to more power usage so use at your own risk.



##### Brightness Control on Monitor
I have a 1440p Dell monitor, and I like to control its brightness/contrast with the Lunar app. This is a complete replacement for the Dell Display Manager on Windows. I can map keys to brightness control which can mimic a MacBook style keyboard.



##### Wi-Fi Adapter (EP-N8508GS)
Before I switched to using Ethernet which works out-of-the-box on macOS with the correct kext, I used to use an EDUP EP-N8508GS USB adapter, which is based on a Realtek chipset not natively supported in macOS. The best fix I have found is <a href="https://github.com/chris1111/Wireless-USB-Big-Sur-Adapter">this tool</a> by chris1111. (you will need to disable SIP temporarily as it installs system extensions, if you are uncomfortable with disabling SIP consider using Ethernet or a <a href="https://dortania.github.io/Wireless-Buyers-Guide/types-of-wireless-card/pcie.html">natively supported PCIe Wi-Fi card.</a>) You can re-enable SIP after installation however. I believe _RtWlanU.kext_ and _RtWlanU1827.kext_ are also required in /EFI/OC/Kexts and your config.plist. I have linked those kexts in the Extras folder in my repository.



##### Bluetoooth
The T3610 does not ship with any kind of bluetooth. However, I use <a href="https://www.amazon.com/IOGEAR-Bluetooth-Multi-Language-Version-GBU521W6/dp/B007ZT2AXE?th=1" rel="nofollow noreferrer">this inexpensive USB bluetooth adapter</a> (not a refferal link) which works in macOS with no additional setup. This adapter also works great in Windows and Linux. AirDrop/Handoff/AirPods switching between devices/whatever Apple feature work properly with this adapter. For less clutter you can put the adapter in the motherboard's internal USB2 port. (provided you mapped the port properly with USBToolBox)



##### AMD RX 550 in macOS
I have the PowerColor version of this card. With no modification you will not gain any graphics acceleration. Assuming it is the only GPU in your system, you should be able to use _RX550-Spoof.aml_ linked in the Extras folder in my repository to spoof the 550 as a supported model (RX 560). If it does not work and you still gain no acceleration (laggy/choppy, no transparency), follow this guide: https://dortania.github.io/Getting-Started-With-ACPI/Universal/spoof.html, using the device ID of an RX 560. Keep in mind some 550s have a vastly different core, so if you continue to not gain any acceleration, you may not be able to use this GPU in macOS due to an incompatibility.



##### Extra Power-Management tips
After following the guide for ssdtPRGen linked below, I like managing power usage stats with Intel Power Gadget, which is a very lightweight usage graph program. It's no longer available for download but it is archived: https://web.archive.org/web/20220701164200/https://www.intel.com/content/dam/develop/external/us/en/documents/downloads/intel-power-gadget.dmg


##### Triple-boot advice with Linux and Windows
I run a triple-boot setup with Windows 11, Linux Mint (formerly Pop!_OS), and macOS. Each OS is in its own SSD. You can install any of these OSes in any order. Windows can be installed normally (with a TPM bypass if running Win11) for the most part, and it should show up in your OpenCore boot picker. For Linux, you will need to put OpenLinuxBoot.efi and ext4_x64.efi (second one is optional) in /EFI/OC/Drivers and your config.plist for proper support. If you install a systemd-boot based flavor such as Pop, it will likely not affect OC, and Linux should just show in the boot picker. However, grub based flavors such as Mint are more likely override your OpenCore configuration in my experience (even when installing to a different EFI parition from OC!), rendering it completely unbootable. To get around this, make sure to always keep a copy of your EFI on a flash drive handy, and once you have installed Linux, if you cannot boot OC, copy your known-good EFI folder onto your hard drive again, replacing all files.








# Thank you for reading!
If this guide helped you, be sure to share it to other people who would benefit from it. If you have a suggestion/question/etc. make an issue and I'll answer it if possible.


  
