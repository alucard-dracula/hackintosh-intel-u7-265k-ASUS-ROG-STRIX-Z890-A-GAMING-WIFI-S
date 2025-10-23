# hackintosh-intel-u7-265k-ASUS-ROG-STRIX-Z890-A-GAMING-WIFI-S

- **简体中文**
- [English](/.github/README-en_us.md)
- [日本語](/.github/README-ja.md)  


**opencore 1.05**  
**support macOS Sequoia 15.0.0 - 26.0.1**  

处理器名称: 8C+12c Intel Core Ultra 7 265K, 5399 MHz (54 x 100)  
主板名称: Asus ROG Strix Z890-A Gaming WiFi S  
主板芯片组: Intel Arrow Point Z890, Intel Arrow Lake-S  

系统内存: 95505 MB  (DDR5 SDRAM)  

显示适配器1: Intel Arrow Lake-HX/S - Integrated Graphics Controller  
显示适配器2: AMD Radeon™ RX 6600  

音频适配器: Realtek USB Audio (Built-in USB Realtek ALC4080 7.1)  
网络适配器: USB Realtek 8125b  
蓝牙适配器: Intel(R) Wi-Fi 7 BE200 320MHz  
~~硬盘驱动器: WD_BLACK SN8100 2000GB  (1863 GB) pcie5.1~~  
硬盘驱动器: WD_BLACK SN850 2000GB  (1863 GB) pcie4.0  

Note:  
macos 没有完全支持SN8100或者PCIE5.1。 虽然能安装系统，但是在频繁写入大批量的小文件时会出现com.apple.filesystems.apfs引起的kernel panic(已测试15和26两个版本的macOS)  

硬件缺陷:  

    1. Intel Arrow Lake 核显无法完美驱动。  

    2. 由于需要仿冒10代intel-cpu, 所以全核心无法跑满， 跑满容易kernel panic  

    3. 外接雷电3扩展坞的AMD Radeon™ RX 6600 无法进入安全模式， 无法支持U盘安装  

    4. ~~由于AppleIGC.kext SimpleGBE.kext 有BUG 会导致kernel panic, 所以内置有线网卡intel-i226-v以及雷电3扩展坞上的 intel-I210有线网卡均无法使用。~~  
     (AppleIGC.kext 1.7.0 已修复该问题)

    5. Intel BE200 无线网卡 无线无法驱动， 蓝牙可以驱动。  


硬件缺陷的解决方案:  

    1. 外接雷电3扩展坞的AMD Radeon™ RX 6600  

    2. 不要长时间跑满CPU核心 （比如 clang -j20） 

    3. 使用恢复版安装 或者 移除雷电3扩展坞后，使用安装专用的EFI（efi-install）使用intel核显进行安装和安全模式操作
    
    4. ~~禁用 AppleIGC.kext SimpleGBE.kext， 外接 USB Realtek 8125b 芯片的有线网卡~~  

    5. 只使用蓝牙，或者外接MacOS26支持的无线网卡  


BISO选项:  

    1. 必须选项：

        1.1. 关闭 原生ASPM （如果不关闭OpenCore无法引导）

    2. 建议选项： 
        
        2.1. 关闭 VT-D 
            （加快MACOS启动速度，减少AMD独显启动黑屏概率。
                不关闭的情况下，修改OpenCore参数或者clear nvram后启动，可能导致长达10-20分钟左右的黑屏或者无法点亮屏幕）

    3. 安装配置（在必须选项的基础上）： 

        3.1. 关闭 UEFI 安全启动  

        3.2. 关闭 快速启动  

        3.3. 关闭 UEFI 运行时变量保护  

        (备注：安装配置的选项, 有网友说不能开启。 
                我的MacOS引导方式是 grub2引导OpenCore, 然后OpenCore再引导MacOS,
                安装完成后再开启，可正常进入MacOS系统)


EFI说明:  

    1. efi-install 进入安全模式的EFI， 只能用于安装升级和维护， 使用intel核显。（需要移除AMD独显）
    
    2. efi-run-system 正常运行系统的EFI 只能用于正常运行系统， 使用AMD独显。（不能进入安全模式, 无法用于安装升级和维护）

U盘安装方法:  

    1. 移除雷电3扩展坞  

    2. 使用efi-install中的OpenCore引导U盘进行安装， 安装过程中请一直使用该EFI进行引导，直到安装完成，跳出MacOS首次设置向导界面。

    3. 跳出MacOS首次设置向导界面后!!!不要!!!进行设置, 直接关机
        （如果设置，系统会非常卡，设置中途退出，可能导致设置不完整系统无法启动。 另外为保护电脑硬件建议按重启按钮后进入BIOS再关闭电源）

    4. 添加雷电3扩展坞  

    5. 使用efi-run-system中的OpenCore引导 已经安装好的系统 进行设置。


升级系统步骤:

    1. 使用efi-run-system中的OpenCore引导进入系统 安装升级包， 直到提示重启。 
    
    2. 重启并关机， 移除雷电3扩展坞（关键时为了移除AMD独显）

    3. 使用 efi-install中的OpenCore引导系统进行升级， 直到跳出登录界面。

    4. !!!不要!!!登录， 直接重启并关机

    5. 连接雷电3扩展坞， 使用efi-run-system中的OpenCore引导进入系统, 并完成设置

    （备注：如果在系统升级的步骤1过程中，经常死机（apfs原生驱动引发kernel panic）可以使用U盘进行升级，
        在升级过程中apfs原生驱动也有可能导致死机，请重试1-2次，如果一直出现请根据自身情况调整OpenCore配置 ）

