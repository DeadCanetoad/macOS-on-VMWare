<div align="center">
    <img src="./images/mac-logo.png" width="30%" />
    <h1>Install macOS on VMware</h1>
</div>

![macOS Desktop](./images/screenshot-0.png)

## Prerequisites

- Download & Install [VMware Workstation](https://www.techspot.com/downloads/189-vmware-workstation-for-windows.html "Download VMware Workstation on TechSpot")
- Download & Extract [VMware Auto Unlocker](https://github.com/paolo-projects/auto-unlocker/releases "Download VMware Auto Unlocker on GitHub")
- Download one of the [macOS ISOs](https://data.pyenb.network/macOS/isos/ "Visit Pyenb macOS ISO repository")

> [!NOTE]  
> VMware Auto Unlocker supports macOS up to version 14.

## How To

### Patching VMware

- Close any VMware Workstation instances
- Open VMware Auto Unlocker `Unlocker.exe` as Administrator
- Because of a bug in the unlocker program ([paolo-projects/auto-unlocker#125](https://github.com/paolo-projects/auto-unlocker/issues/125)), you need to do this:

> [!IMPORTANT]  
> If you've already patched VMware Workstation before, uninstall the patch first in the unlocker program.

- Download both `darwin.iso` & `darwinPre15.iso` from <https://packages.vmware.com/tools/frozen/darwin/> and put them in a folder
- In the unlocker program, uncheck `Download tools` and browse to the directory containing the files you've downloaded
- Patch VMware Workstation!

### Creating macOS VM

- Open VMware Workstation and click continue as free non-commercial use
- Create a VM:
  - **Configuration:** Custom
  - **Hardware Compatibility:** Default (Workstation 17.5.x)
  - **Image File:** Point to the ISO file you've downloaded
  - **OS:** Apple Mac OS X
  - **Version:** Depends on what macOS version you've downloaded
  - **Name & Location:** Whatever you like. Remember the name & the path!
  - **Processors:** Default or more (depends on your system)
  - **RAM:** 2 GB or more (depends on your system)
  - **Network:** Default (NAT)
  - **I/O:** Default (LSI Logic)
  - **Virtual Disk:** Default (SATA)
  - **Disk:** Create New. 35 GB or more. Split Virtual Disk. Default Name.
  - 🤜 Finish it!

- Remember the location of your VM that you've just created?  
  Open `{Your VM Name}.vmx` in a text editor:
  - Set `board-id.reflectHost = "FALSE"`
  - Set `ethernet0.virtualDev = "vmxnet3"`
  - Add:
    ```
    board-id = "Mac-AA95B1DDAB278B95"
    hw.model.reflectHost = "FALSE"
    hw.model = "MacBookPro19,1"
    serialNumber.reflectHost = "FALSE"
    serialNumber = "C01234567890"
    ```
  - For running on an AMD CPU, add the following to the .vmx file:
    ```
    smc.version = "0"
    cpuid.0.eax = "0000:0000:0000:0000:0000:0000:0000:1011"
    cpuid.0.ebx = "0111:0101:0110:1110:0110:0101:0100:0111"
    cpuid.0.ecx = "0110:1100:0110:0101:0111:0100:0110:1110"
    cpuid.0.edx = "0100:1001:0110:0101:0110:1110:0110:1001"
    cpuid.1.eax = "0000:0000:0000:0001:0000:0110:0111:0001"
    cpuid.1.ebx = "0000:0010:0000:0001:0000:1000:0000:0000"
    cpuid.1.ecx = "1000:0010:1001:1000:0010:0010:0000:0011"
    cpuid.1.edx = "0000:0111:1000:1011:1111:1011:1111:1111"
    ```

- 💾 Save it

### Installing macOS

- Power On your VM!
- Wait until all initialization is complete
- Select your language
- Select Disk Utility
- Select the `VMware Virtual SATA Hard Drive Media` > Click Erase
- Name the drive > Format to APFS > Erase
- Click Done > Close ❌ the Disk Utility
- Install macOS. The VM will be rebooted several times
- Follow the typical OS installation (Basically **Not Now** or **Skip**)
- You're done! Enjoy 😉

![macOS Info](./images/screenshot-1.png)

## Credits

- https://i12bretro.github.io/tutorials/0602.html/
- https://github.com/kiraio-moe/macOS-on-VMWare/
- https://etechbox.com/download/macos-vmware-amd/