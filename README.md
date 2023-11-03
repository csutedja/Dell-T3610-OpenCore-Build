# Dell Precision T3610 OpenCore Build
Guide on how to get up to macOS Big Sur almost fully working on a Dell Precision T3610 workstation. 

Before you proceed:


⚠ WARNING! Most likely, you cannot and will never be to run macOS Monterey or higher (12.0+) on this machine. This is essentially because the T3610 motherboard lacks the ability to perform a TSC reset, and the CPU also lacks a certain register, read more here: https://github.com/acidanthera/CpuTscSync#cputscsync


⚠ This is **not** a pre-made EFI folder, I am instead providing guidance on what files/settings/folder structure to use. This is because kexts/OpenCore update all the time, and I don't want to provide outdated files.  


What works:
- GPU acceleration (I use a PowerColor AMD RX 550 which is not natively supported, I will provide a .ssdt file to get that working if you have that card)
- App Store
- iMessage/FaceTime (requires generated SMBIOS; if you are unlucky like me you will have to call Apple to approve your serial number)
- Ethernet
- WiFi (will vary depending on your dongle/wireless card)
- Sound (built-in speaker and through GPU)
- Bluetooth (through a USB dongle)
- DRM (AppleTV/Netflix/etc.)
- 4k display
- Software Updates 

What doesn't work:
- macOS versions past Big Sur (read above)
- Built-in USB 3.0 (use an expansion card if needed)
- Sleep (supposedly works with modded BIOS but I find that too risky personally) 
- Sidecar (not tested)

  
