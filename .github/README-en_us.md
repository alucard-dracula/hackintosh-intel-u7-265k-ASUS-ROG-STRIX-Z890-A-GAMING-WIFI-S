# hackintosh-intel-u7-265k-ASUS-ROG-STRIX-Z890-A-GAMING-WIFI-S

- [简体中文](/./README.md)
- **English**
- [日本語](/.github/README-ja.md)


**OpenCore 1.05**
**Supports macOS Sequoia 26.0.0 - 26.0.1**

Processor: 8P+12E Intel Core Ultra 7 265K, 5399 MHz (54 x 100)
Motherboard: Asus ROG Strix Z890-A Gaming WiFi S
Chipset: Intel Arrow Point Z890, Intel Arrow Lake-S

System Memory: 95505 MB (DDR5 SDRAM)

Graphics Adapter 1: Intel Arrow Lake-HX/S - Integrated Graphics Controller
Graphics Adapter 2: AMD Radeon™ RX 6600

Audio Adapter: Realtek USB Audio (Built-in USB Realtek ALC4080 7.1)
Network Adapter: USB Realtek 8125b
Bluetooth Adapter: Intel(R) Wi-Fi 7 BE200 320MHz
Storage Drive: WD_BLACK SN8100 2000GB (1863 GB)

Known Hardware Issues:

1. The Intel Arrow Lake integrated GPU cannot be perfectly driven.

2. Because the CPU must be spoofed as a 10th-generation Intel CPU, the full cores cannot be run at full load; running all cores at full load may cause a kernel panic.

3. The external Thunderbolt 3 dock's AMD Radeon™ RX 6600 cannot boot into Safe Mode and does not support USB installer usage.

4. Due to bugs in AppleIGC.kext and SimpleGBE.kext that can cause kernel panics, the built-in wired NIC intel-i226-v and the Intel I210 on the Thunderbolt 3 dock cannot be used.

5. The Intel BE200 Wi‑Fi adapter cannot be driven for wireless; Bluetooth works.


Workarounds for Hardware Issues:

1. Remove or avoid using the external Thunderbolt 3 dock with the AMD Radeon™ RX 6600 when performing operations that require Safe Mode or USB installers.

2. Avoid running the CPU at full sustained load for long periods (for example, avoid running builds like clang -j20 continuously).

3. Use the recovery-mode installer or remove the Thunderbolt 3 dock and use the installer-specific EFI (the `efi-install` folder) to install and perform Safe Mode operations using the Intel integrated graphics.

4. Disable `AppleIGC.kext` and `SimpleGBE.kext` and use an external USB wired NIC based on the Realtek 8125b chipset.

5. Use Bluetooth only, or connect an external Wi‑Fi adapter that is supported by macOS 26.


BIOS Settings:

1. Required settings:

    1.1. Disable native ASPM (if not disabled OpenCore will not boot).

2. Recommended settings:

    2.1. Disable VT‑d
        (This speeds up macOS boot and reduces the chance of a black screen when the AMD discrete GPU initializes. If not disabled, modifying OpenCore settings or doing a clear NVRAM may cause a 10–20 minute black screen or failure to light the display.)

3. Installation-related settings (in addition to required settings):

    3.1. Disable UEFI Secure Boot

    3.2. Disable Fast Boot

    3.3. Disable UEFI runtime variable protection

    (Note: Some users report they cannot enable these settings during installation. My macOS is booted by grub2 which then chains to OpenCore, and I enable these settings again after installation—this works for me.)


EFI Notes:

1. `efi-install` is an EFI configured for entering Safe Mode and should only be used for installation, upgrades, and maintenance. It uses the Intel integrated graphics (you must remove the AMD discrete GPU).

2. `efi-run-system` is an EFI used for normal system operation and uses the AMD discrete GPU (it cannot boot into Safe Mode and should not be used for installation, upgrades, or maintenance).


USB Installer Steps:

1. Remove the Thunderbolt 3 dock.

2. Use the OpenCore on the USB drive from the `efi-install` folder to boot and install. During installation continue to use that EFI until the installation finishes and the macOS first-run setup assistant appears.

3. When the macOS first-run setup assistant appears, DO NOT complete the setup—shut down immediately.
    (If you proceed with setup, the system will be extremely slow and an incomplete or interrupted setup may leave the system unbootable. To protect hardware, it's recommended to press the restart button and then enter BIOS before powering off.)

4. Reconnect the Thunderbolt 3 dock.

5. Use the OpenCore from the `efi-run-system` folder to boot the installed system and complete the setup.


System Upgrade Steps:

1. Boot into the installed system using the OpenCore in `efi-run-system`, install the upgrade package, and follow prompts until the system requests a restart.

2. Restart and then power off. Remove the Thunderbolt 3 dock (this is critical to remove the AMD discrete GPU).

3. Boot the system using the OpenCore in `efi-install` to perform the upgrade, continue until the login screen appears.

4. DO NOT log in—restart and power off immediately.

5. Reconnect the Thunderbolt 3 dock and boot the system using the OpenCore in `efi-run-system`, then complete any remaining setup steps.

(Note: If the system frequently crashes during step 1 of the upgrade due to APFS native driver kernel panics, you can perform the upgrade from a USB installer. APFS native drivers can also cause crashes during upgrades—retry 1–2 times, and if crashes persist, adjust OpenCore configuration accordingly.)
