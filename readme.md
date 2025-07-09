## 非品牌机
- [Non-branded\Tools](https://github.com/Charles-Miao/ManufacturingTestEngineerLib/tree/master/Non-branded/Tools)：Start is the main program for Runin Test & Function Test
- [Non-branded\Tools64\Route](https://github.com/Charles-Miao/ManufacturingTestEngineerLib/tree/master/Non-branded/Tools64/Route)：OnRack is the main program for Runin DL & SW DL
- [Non-branded\Path](https://github.com/Charles-Miao/ManufacturingTestEngineerLib/tree/master/Non-branded/Path): OnRack calls WINRUNIN and WINSWDL, then calls the RUNINDL and SWDL programs in %ProjectName%

## 品牌机

### Server
- 创建共享文件夹
- DHCP配置
- [WDS配置](https://github.com/Charles-Miao/ManufacturingTestEngineerLib/tree/master/Branded/Server/WDS配置.md)

### WinPE Image制作
- [WinPE: Mount and Customize](https://learn.microsoft.com/en-us/windows-hardware/manufacture/desktop/winpe-mount-and-customize?view=windows-11)

```batch
装载 Windows PE 启动映像
Dism /Mount-Image /ImageFile:"C:\WinPE_amd64\media\sources\boot.wim" /index:1 /MountDir:"C:\WinPE_amd64\mount"

添加WinPE-MDAC.cab 文件
Dism /Add-Package /Image:"C:\WinPE_amd64\mount" /PackagePath:"C:\Users\WT\Desktop\FBMFGPE\WinPE-MDAC.cab"  

卸载并保存映像
Dism /Unmount-Image /MountDir:"C:\WinPE_amd64\mount" /commit

卸载不保存映像
Dism /Unmount-Image /MountDir:"C:\WinPE_amd64\mount" /discard

# 查看已安装的所有包
DISM /Image:"C:\Mount\WinPE" /Get-Packages

# 查看所有已挂载的映像
DISM /Get-MountedImageInfo

# 安装WinPE-MDAC.cab，既有odbcad32工具，此工具可以操作SQL Server
odbcad32
```
- **WinPE-MDAC**：支持 Microsoft 开放式数据库连接 (ODBC)、OLE DB 和 Microsoft ActiveX 数据对象 (ADO)。 这套技术提供对各种数据源（例如 Microsoft SQL Server）的访问。 例如，通过这种访问可以查询包含 ADO 对象的 Microsoft SQL Server 安装。 可以基于唯一的系统信息生成动态应答文件。 同样，可以生成数据驱动的客户端或服务器应用程序，用于集成来自各种数据源（关系型 (SQL Server) 和非关系型）的信息。
- **WinPE-MDAC**：可以从Windows ADK中获取或者从其他Wim文件中提取 

### SWDL1
#### startnet.cmd call ProductName.cmd
- 初始化设定/联网/拷贝服务器资料等
- 调起ProductName.cmd

#### ProductName.cmd call Check.cmd
- 使用wmic和ReadSMBIOS.exe获取机种名，以便拷贝正确目录%ProductName%下的Check.cmd
- 调起Check.cmd

#### [Check.cmd](https://github.com/Charles-Miao/ManufacturingTestEngineerLib/tree/master/Branded/ATOTools/Project/check.cmd.md)
- Add-TPDriver，使用pnputil.exe安装
- Get PCBA SN from DUT，使用'Wmic Baseboard Get SerialNumber /Value'读取
- Get_BOM，使用CURL获取
- ATOSetLogPath，将log目录设定到服务器上
- CheckACIN，通过'Wmic path win32_battery Get batterystatus /Value'检查机台是否插AC
- Get BIOS/EC Version，通过Wmic接口读取机台BIOS/EC并记录到log中
- LIDCLOSE，使用DumpIO.exe设定盒盖不休眠
- MFG Check/ SetToMFGMode，使用DumpIO.exe设定MFG模式
- settarget，使用DumpIO.exe设定目标电量为40
- enterCALIBRATION，使用DumpIO.exe设置电池RSOC校准模式
- diskUnsafeShutdowns，smartctl.exe记录磁盘信息，diskinfo.exe读取不安全关机次数，并记录到log中
- ATOVer，Get ATO version(测试image版本)
- DLATO，服务器没有ATO image，则记录到log中，并暂停
- PartitionDisk，格式化
- Install image，通过DISM.exe install image
- BCDBOOT，执行设置启动项命令
- Reagentc，设置Windows恢复环境镜像
- bcdedit，设置测试签名模式为开启/设置恢复功能为禁用/设置重启失败后自动重启
- reg，添加系统首次启动后运行的任务
- Update，把image中Update内容和RunOnce.cmd拷贝到系统中
- MOPSPEDL2，复制MOPS PE文件到指定目录
- Copy_EFI，复制bootmgfw.efi文件到指定位置
- 拷贝log，并reboot

### FAT+FRT
#### RunOnce.cmd
- Start Install_Update.cmd
```batch
:Uninstall_cyusb3 REM 使用 pnputil 工具强制卸载指定的 cyusb3 驱动
:Reinstall_cyusb3 REM 使用 pnputil 工具安装指定路径下的 cyusb3 驱动
:install_CH341WDM REM 使用 pnputil 工具安装指定路径下的 CH341WDM 驱动
:Disabel_Defender_Notificationy REM regedit静默执行指定路径下的注册表文件
```
- 使用 devcon.exe 工具移除所有 SCSI 磁盘设备驱动
- 以远程签名执行策略运行 PowerShell 脚本，禁用 Windows 更新任务计划
- Addstartup，向注册表 RunOnce 项添加键值，使系统下次启动时运行 C:\WTtools\GetTools.cmd
- WallpaperSet，C:\WTtools\image\WallpaperSet.cmd 文件存在，则调用该脚本
```batch
REM 获取 BIOS 版本信息
REM 使用 magick.exe 工具，在 img0.jpg 图片的 (250, 341) 位置以红色 40 号字体添加 BIOS 版本信息，生成新图片 img0_new.jpg
REM 以无配置文件和绕过执行策略的方式运行Set-Wallpaper.ps1脚本，设置桌面壁纸
```
```POWERSHELL
# Set-Wallpaper.ps1
# 定义了一个名为Set-Wallpaper的函数(C#)，用于设置Windows系统的桌面壁纸，最后调用该函数将指定路径的图片设置为桌面壁纸
```
- WTIO.exe /w，无特别注释
- SSDUnsafe，执行 diskinfo.exe 工具，获取磁盘不安全关机次数，如果大于80则fail
- SaveConsoleAsText.ps1，主要功能是将控制台的输出内容保存到 C:\WTTools\DL1.log 文件中
- 强制重启计算机

#### GetTools.cmd
- GETDHCP，获取DHCP Server IP
- net config workstation 命令，获取用户名是否为 "WT"
- ipconfig /all ^|find "Ethernet adapter Ethernet"，获取网卡名称
- netsh advfirewall set publicprofile state off，关闭公共网络配置文件的防火墙
- ProductName，用于获取产品名称
- net use/ net time，连接服务器，同步时间
- Get PCB SN from machine by WMI
- DLATOTools，将testtool目录中的所有内容拷贝到桌面上，将部分自动化工具拷贝到C盘根目录
- Start FAT.bat

#### FAT.bat

### Burn-in LED

### FFT自动测试 

### RF
- [测试原理和调试方法](https://github.com/Charles-Miao/ManufacturingTestEngineerLib/tree/master/Branded/RF/测试原理和调试方法.md)

### Audio

### SWDL2

### OA3

### MES交互逻辑&卡控