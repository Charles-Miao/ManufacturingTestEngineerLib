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
rem 装载 Windows PE 启动映像
Dism /Mount-Image /ImageFile:"C:\WinPE_amd64\media\sources\boot.wim" /index:1 /MountDir:"C:\WinPE_amd64\mount"
rem 添加WinPE-MDAC.cab 文件
Dism /Add-Package /Image:"C:\WinPE_amd64\mount" /PackagePath:"C:\Users\WT\Desktop\FBMFGPE\WinPE-MDAC.cab"  
rem 卸载并保存映像
Dism /Unmount-Image /MountDir:"C:\WinPE_amd64\mount" /commit
rem 卸载不保存映像
Dism /Unmount-Image /MountDir:"C:\WinPE_amd64\mount" /discard
rem 查看已安装的所有包
DISM /Image:"C:\Mount\WinPE" /Get-Packages
rem 查看所有已挂载的映像
DISM /Get-MountedImageInfo
rem 安装WinPE-MDAC.cab，既有odbcad32工具，此工具可以操作SQL Server
odbcad32
```
- **WinPE-MDAC**：支持 Microsoft 开放式数据库连接 (ODBC)、OLE DB 和 Microsoft ActiveX 数据对象 (ADO)。 这套技术提供对各种数据源（例如 Microsoft SQL Server）的访问。 例如，通过这种访问可以查询包含 ADO 对象的 Microsoft SQL Server 安装。 可以基于唯一的系统信息生成动态应答文件。 同样，可以生成数据驱动的客户端或服务器应用程序，用于集成来自各种数据源（关系型 (SQL Server) 和非关系型）的信息。
- **WinPE-MDAC**：可以从Windows ADK中获取或者从其他Wim文件中提取 

### SWDL1
#### startnet.cmd call ProductName.cmd
```batch
rem 初始化设定/联网/拷贝服务器资料等
rem 调起ProductName.cmd
```

#### ProductName.cmd call Check.cmd
```batch
rem 使用wmic和ReadSMBIOS.exe获取机种名，以便拷贝正确目录%ProductName%下的Check.cmd
rem 调起Check.cmd
```

#### [Check.cmd](https://github.com/Charles-Miao/ManufacturingTestEngineerLib/tree/master/Branded/LH/ATOTools/Project/check.cmd.md)
```batch
:Add-TPDriver rem 使用pnputil.exe安装
rem Get PCBA SN from DUT，使用'Wmic Baseboard Get SerialNumber /Value'读取
:Get_BOM rem 使用CURL获取
:ATOSetLogPath rem 将log目录设定到服务器上
:CheckACIN rem 通过'Wmic path win32_battery Get batterystatus /Value'检查机台是否插AC
rem Get BIOS/EC Version，通过Wmic接口读取机台BIOS/EC并记录到log中
:LIDCLOSE rem 使用DumpIO.exe设定盒盖不休眠
rem MFG Check/ SetToMFGMode，使用DumpIO.exe设定MFG模式
:settarget rem 使用DumpIO.exe设定目标电量为40
:enterCALIBRATION rem 使用DumpIO.exe设置电池RSOC校准模式
:diskUnsafeShutdowns rem smartctl.exe记录磁盘信息，diskinfo.exe读取不安全关机次数，并记录到log中
:ATOVer rem Get ATO version(测试image版本)
:DLATO rem 服务器没有ATO image，则记录到log中，并暂停
:PartitionDisk rem 格式化
rem Install image，通过DISM.exe install image
:BCDBOOT rem 执行设置启动项命令
:Reagentc rem 设置Windows恢复环境镜像
:bcdedit rem 设置测试签名模式为开启/设置恢复功能为禁用/设置重启失败后自动重启
:reg rem 添加系统首次启动后运行的任务
:Update rem 把image中Update内容和RunOnce.cmd拷贝到系统中
:MOPSPEDL2 rem 复制MOPS PE文件到指定目录
:Copy_EFI rem 复制bootmgfw.efi文件到指定位置
rem 拷贝log，并reboot
```

### FAT+FRT
#### RunOnce.cmd
```batch
Start Install_Update.cmd
rem 使用 devcon.exe 工具移除所有 SCSI 磁盘设备驱动
rem 以远程签名执行策略运行 PowerShell 脚本，禁用 Windows 更新任务计划
:Addstartup rem 向注册表 RunOnce 项添加键值，使系统下次启动时运行 C:\WTtools\GetTools.cmd
:WallpaperSet rem C:\WTtools\image\WallpaperSet.cmd 文件存在，则调用该脚本
rem WTIO.exe /w，无特别注释
:SSDUnsafe rem 执行 diskinfo.exe 工具，获取磁盘不安全关机次数，如果大于80则fail
rem SaveConsoleAsText.ps1，主要功能是将控制台的输出内容保存到 C:\WTTools\DL1.log 文件中
rem 强制重启计算机
```

```batch
rem Install_update.cmd
:Uninstall_cyusb3 REM 使用 pnputil 工具强制卸载指定的 cyusb3 驱动
:Reinstall_cyusb3 REM 使用 pnputil 工具安装指定路径下的 cyusb3 驱动
:install_CH341WDM REM 使用 pnputil 工具安装指定路径下的 CH341WDM 驱动
:Disabel_Defender_Notificationy REM regedit静默执行指定路径下的注册表文件
```

```batch
rem WallpaperSet.cmd
REM 获取 BIOS 版本信息
REM 使用 magick.exe 工具，在 img0.jpg 图片的 (250, 341) 位置以红色 40 号字体添加 BIOS 版本信息，生成新图片 img0_new.jpg
REM 以无配置文件和绕过执行策略的方式运行Set-Wallpaper.ps1脚本，设置桌面壁纸
```
```POWERSHELL
# Set-Wallpaper.ps1
# 定义了一个名为Set-Wallpaper的函数(C#)，用于设置Windows系统的桌面壁纸，最后调用该函数将指定路径的图片设置为桌面壁纸
```

#### GetTools.cmd
```batch
:GETDHCP rem 获取DHCP Server IP
rem net config workstation 命令，获取用户名是否为 "WT"
rem ipconfig /all ^|find "Ethernet adapter Ethernet"，获取网卡名称
rem netsh advfirewall set publicprofile state off，关闭公共网络配置文件的防火墙
:ProductName rem 用于获取产品名称
rem net use/ net time，连接服务器，同步时间
rem Get PCB SN from machine by WMI
:DLATOTools rem 将testtool目录中的所有内容拷贝到桌面上，将部分自动化工具拷贝到C盘根目录
Start FAT.bat
```

#### [FAT.bat](https://github.com/Charles-Miao/ManufacturingTestEngineerLib/tree/master/Branded/LH/Testtool/ProjectName/FAT.md)
```batch
:UnzipATS rem 若当前脚本所在目录下存在 ATS.zip 文件，则调用 7z.exe 解压到 C 盘根目录，覆盖已有文件
:PCBSN rem 使用wmic获取PCBA SN
:STATIONRESULT rem 设定测试结果文件路径
:TargetIP rem 获取DHCP Server IP
:GetStation rem 向 MES 系统发送 POST 请求，查询测试站记录，并将%Station%设定为对应站别[FAT/FRT/FFT/SWDL]
:GetToolVer rem 从mes回复的信息中获取工具版本号
:GetTool rem 删除tool目录，重新解压%toolver%.7z
:DeleteData rem 删除tool生成的相关数据
:IfFRTFFTSWDL rem 如果是FRT/FFT/SWDL站别，则调起FRT.bat/FFT.bat/SWDL.bat
:FloatMode rem 用于设置浮动模式
:DisableLid rem 用于禁用盖子功能
rem 删除所有无线局域网配置文件
call NoteBookTest.exe N69528_FAT
```

#### NoteBookTest.exe N69528_FAT
- FlashBiosTest 
```C#
//检查 AC 状态，WtDevTool.exe -GetBatteryPowerInfo
//确认离线模式，读取配置档mMainCfg.AbsoluteSysCfg.MESCfg.EnableOnLine
//获取电池电量百分比，WtDevTool.exe -GetBatteryPowerInfo
//设置电池浮动模式，开启工厂模式，通过inpout32.dll设定对应EC Address的Data
//获取本地 BIOS 版本信息，WtDevTool.exe -CheckSMBIOS
//flashBiosCount是记录 BIOS 刷写次数的文件
//DeleteRunExe，删除启动项"C:\ProgramData\Microsoft\Windows\Start Menu\Programs\StartUp"
//DownloadBiosBin，创建 FTP 客户端实例FtpClient ftp（连接，获取列表，下载，断开连接）
//SetRunExe，将现有正在执行的NoteBook.exe设定为开机启动项
//执行OtherTool\BiosUnlock\Bios_unLock.exe
//执行下载的BIOS文件，OtherTool\FlashBios\*.exe
```
- Disktest
```C#

```
- MainBoardTest
- METest
- TPMTest
- FanTest
- KeyboardTest
- ClickpadTest
- LCDTest
- CameraTest
- BatteryTest
- WifiTest
- BluetoothTest
- FingerPrintTest
- Touchpanel test
- WriteNumber

#### FRT.bat
```batch
rem 获取toolver，并检查tool目录是否存在
:CheckProcess rem 检查notebook.exe进程，如果存在则检查30次，否则结束此进程（逻辑奇怪）
rem 清除系统事件日志
:OEMString3 rem oemstring不管是不是“FBMTL”，都调起一样的工具（逻辑奇怪）
call NoteBookTest.exe N69528_FRT_4H
```

#### NoteBookTest.exe N69528_FRT_4H
- BatteryTest
- ColdBootOneTimeTest
- MemoryTest
- CheckEcTest
- Flash Blos
- ParallelTest
- ColdBootTest
- CPUTest/S3Test/S4Test
- GraphicsTest
- FanTest
- OledcheckSumTest
- ShutdownCheck
- CheckTimeSequend

### Burn-in LED

### FFT
#### FFT.bat
```batch
rem 获取toolver，并检查tool目录是否存在
:FloatMode rem 定义进入电池浮动模式
:ExitLidTestMode rem 退出盖子测试模式
:SetFanTestMode rem 进入风扇测试模式，设置风扇转速
rem 强制删除注册表中指定路径下的键值
rem igc.exe，用途未知
call NoteBookTest.exe N69528_FFT
rem 退出风扇测试模式
```

#### NoteBookTest.exe N69528_FFT
- 静音房
```plaintext
FanTest
NoiseTest
```
- LCD AOI
```plaintext
程式与ITC_AOI.exe交互测试
DUT打开测试工具，通过二维码wifi连接设备
DUT分别切换白灰黑，红绿蓝画面,设备进行拍照分析
```
- LCD Mura
```plaintext
程式与ITC_Mura.exe交互测试
设备相机识别DUT桌面二维码获取产品信息进行测试
通过WIFI指令分别切屏，进行90°垂直平面和45°斜拍测试
使用成像色度仪进行计算
```
- LCD色域
```plaintext
程式与ITC_CCA.exe交互测试
设备相机识别DUT桌面二维码获取产品信息进行测试
产品切104张图片并同时切换到Native状态运行荣耀色域算法计算
测试完成将算法计算的数据写入产品BIOS
```
- 指纹自动化测试
```plaintext
程式与FPCProductionTestTool.exe交互测试
设备相机识别DUT桌面二维码开始测试
设备的假手指移动到开机按钮触摸指纹
```
- 摄像头测试
```plaintext
程式与AutoWebCam.exe交互测试
DUT工具分析拍摄结果
```
- 键盘测试
```plaintext
程式与KeyboardTP.exe交互测试
设备相机模组识别DUT桌面二维码获取键盘国别信息并开始测试
设备机械滚轮分别从左上到右下依次滑过DUT键盘的每个键
```
- 触摸板功能测试
```plaintext
程式与GFMPTest.exe交互测试
机械手从左往右从上至下依次划十字形，工具接收到触碰板反馈的X,Y轴10组不同的值
机械手按压左右按键/空格键左中右三个区域，通过判定划线长度大于1000和按键scancode判断功能是否正常
```
- LED测试
```plaintext
通过WT_LEDTEST.exe交互测试
根据项目配置表是否有键盘背光 ，自动配置工具
设备相机模组识别DUT桌面二维码，发送指令开始测试
程序控制打开DUT灯，设备相机拍摄，灯灭灯亮图片进行解析
```
- NFC测试
```plaintext
DUT进入夹具，夹具光感器识别到DUT到位后，工具开始测试
通过读卡器读取NFC已写入的数据，上机位扫描DUT SN,从MES中获取老化写入的NFC字符和源数据作比对
```
- TouchPanel测试
- Typce-C测试
- USB3.0测试
- HDMI测试
- AudioJack测试
- HallsensorTest
```plaintext
OP合盖起来打开是否可以正常唤醒
```
- LCDTest
```plaintext
1.工具自动调整背光亮度，从0到100以20%递增，100到0以30%递减，人员观察
2.RGB测试：红绿蓝白黑五种画面变化，图片变化，人员观察
```

#### RF
- [测试原理和调试方法](https://github.com/Charles-Miao/ManufacturingTestEngineerLib/tree/master/Branded/NoteBook/RF/测试原理和调试方法.md)

#### Audio
- 使用ITCTestAudio4Laptop.exe程序与厂商音频测试设备进行交互测试

### SWDL2
#### SWDL.bat
```batch
rem 获取toolver，并检查tool目录是否存在
:CopyFFTLog rem 复制FFT日志
:FloatMode rem 进入浮动模式
rem Wmic Baseboard Get SerialNumber /Value, 读取PCBSN
:GetWO rem 获取工单信息
call NoteBookTest.exe N69528_SWDL/ N69528_SWDL_NoWriteOA3 rem 写入or不写OA3
```

#### N69528_SWDL
- MES Station Check
```plaintext
SWDL工具读取主板中的MBSN
通过MES接口获取对应的ASSY SN
通过ASSY SN获取MES中SWDL站点状态
如果获取SWDL站点状态信息是pass,则去获取工单配置数据
```
- WriteNumberCheck
```plaintext
WriteNumberCheck
MainboardTest
TPMTest
METest
FanTest
CheckTimeSequence
WriteNumber(Write OA3)
```
- SWDL2 start
```plaintext
核对项都Pass，设置PXE启动，再Preload切换到WinPE
A_SWDL 结束后reboot ，进入PXE，下载WinPE
```
- CFG check
```plaintext
check EC MFG mode
battery 电量管控
Check station
Config Check
open MFGmode
flash BIOS
Check ME lock
BIOS_load_default
```
- Get Preload version
```plaintext
通过SN在MES上获取对应SN的Preload版本信息
```
- Preload Image
```plaintext
通过preload版本信息从服务器下载对应版本配置文件preload version Bom_XML
逐行解析Bom_XML获取所有module names,在服务器上下载对应modules
使用DISM install image
```
- OA3 Process
```plaintext
调用oa3tool.exe /CheckEdition 检查OA3 key 与OS edition 是否匹配
oa3tool.exe 生成OA3.xml
MES_OA3_WI90119 获取OA3 ProductKeyID 与机器生成的OA3.xml 里的ProductKeyID 比对
OA3.xml 到Preload 服务器
调用MES_OA3_WI90134 check HardHash status
```
- Shipping Setting
```plaintext
进Audit mode安装driver,app,office，设置OOBE,电源选项,startlayout,电量管控（出货管控电量为50%-85%）,Onekey,PBR等，修改BCD 下次开机进MFG PE
检测preload 结果
lid enable
Shipping mode setting
EC EXIT MFG MODE
check SecureBoot
```
- SWDL2 MES 过站
```plaintext
CHK_Barcode 显示屏显示电池电量（出货管控电量为50%-85%），整机二维码，下架人工扫描二维码和流程卡二维码一致pass过站，转到包装线（二维码内容：抽检标识 OBE/PASS+SN）
show 工单号，相关配置信息
```

#### startnet.cmd
```batch
call autorun.cmd
```

#### autorun.cmd
```batch
:SWDL2 rem call connectserver.cmd获取dhcp ip，连接server，同步时间
rem Wmic bios Get SerialNumber /Value, 通过 WMI 获取系统序列号
rem ImageWhitelist.exe，获取%Whitelist%机种名称
call BOM.bat
if exist c:\oem\MFGPE.tag goto MFGPE2？
:MFGPE1 rem 拷贝服务器脚本-->遍历目标目录中所有的*.cmd，并执行DUT_Config_Check_0625-cxl.cmd %whiteList%-->call pcw_preload.exe preloader.ini-->goto pass
:MFGPE2 rem 拷贝服务器脚本-->遍历目标目录中所有的*.cmd，并执行DUT_Config_Check_0625-cxl.cmd-->拷贝QRcode_check-->删除C:\sources,C:\temp,C:\AMD,C:\HW*,C:\oem,隐藏C:\PerfLogs,C:\Data
:FACLAST rem shipmode.cmd-->MFGDONE.cmd, fail call exitshipmode.cmd-->QRcode.cmd, fail call exitshipmode.cmd
shutdown
```
```batch
rem shipmode.cmd？
rem %MESPSI_Injection%不等于Y，也不等于N，goto setshipmode
rem %MESPSI_BIOS_FLAG%不等于空，则继续执行
HUAWEIPSI.exe -w %MESPSI_BIOS_FLAG%  REM 调用 HUAWEIPSI.exe 工具，将 MESPSI_BIOS_FLAG 的值写入
HUAWEIPSI.exe -r rem 确认MESPSI_BIOS_FLAG正常写入
:setshipmode rem 使用DumpIO.exe将机台设定为shipmode
```
```batch
rem MFGDONE.cmd
:SecureBoot rem 确认SecureBoot Enable，则goto MFGMODE
:EXITMFGMODE rem 使用DumpIO.exe退出制造模式
```
```batch
rem QRcode.cmd
:PXETOHDD
Wmic Baseboard Get SerialNumber /Value rem 获取MBSN
Wmic bios Get SerialNumber /Value rem 获取整机SN
:Get_MesLine rem 透过mes获取line信息
:Update_SFC rem 透过mes获取测试结果SFC_OK/SFC_OK_SOBE/SFC_OK_FOBE，赋值QRcodeMSG=PASS/SOBE/FOBE
x:\QRcode_check\QRcode_check.exe !QRcodeMSG!,%FGSN%3,%FGSN%,%WO%,%productName%  REM 调用 QRcode_check.exe 工具，显示QRcodeMSG/FASN/WO/ProductName等信息
:Lid_Close_exit rem 使用DumpIO.exe工具设定lid close mode reopen
```
```batch
rem exitshipmode.cmd
:exitshipmode rem 使用DumpIO.exe退出shipmode
```

#### DUT_Config_Check_0625-cxl.CMD
```batch
:checkACIN rem 检查 AC 电源状态
:Add_PEDriver rem 添加 WinPE 驱动
:GetSYSInfo rem 使用 Wmic 命令获取主板序列号，并赋值给 PCBSN
:GetBIOS_Ver rem 通过 WMI 获取 BIOS 版本
:GetEC_Ver rem 通过 WMI 获取 EC 版本
:Get_BOM rem 获取 BOM 信息
:If_AfterFlashBIOS_or_AfterLockME rem 检查是否为刷 BIOS 或锁 ME 后的操作 (如果存在MElock.flg or flashbios.flg，则将log追加到preload.log中，最后将preload.log重命名为preload%RANDOM%.log)
:SetLog_PATH rem 设置log路径，并start BatteryControl.bat(每隔180秒，在log中记录一次电量)
:Preparation rem 删除一些中间文件，并生成facMode.flg
:If_AfterFlashBIOS_or_AfterLockME rem 检查是否为刷 BIOS 或锁 ME 后的操作 (如果存在MElock.flg or flashbios.flg，则将log追加到preload.log中，最后将preload.log重命名为preload%RANDOM%.log)
:LIDCLOSE rem 调用DumpIO.exe退出shipping mode/Force charging mode/Force Discharging mode/Stop Charge and Discharge mode
:checkMFG rem 检查制造模式状态，如果是制造模式，则跳过SetToMFGMode
:SetToMFGMode rem 设置制造模式
:settarget rem 设置目标电池容量80
:enterCALIBRATION rem 进入电池校准模式
:Getbattery rem 获取当前电池容量
:Station_check rem SWDL2 工位检查
:CHKINFO rem 检查系统 SN 与 MES 获取的 SN 是否匹配
:OA3ExistCheck rem 流程图参见https://github.com/Charles-Miao/ManufacturingTestEngineerLib/tree/master/Branded/DL2/Shipping_Image/ProjectName/MEStools/AMD64/MOPSPE/OA3ExistCheck.md，围绕MES回复的3个参数returnmsftpkid,returnort,islink，确认SET OA3_Exist=YES？
:SMBIOSCHECK rem smbios检查并与mes SMBIOS_TYPE2_Product/SMBIOS_TYPE11_String1/SKUNumber_MES比对
:SKUNumber_PreloadType rem 获取PreloadType，并与mes SMBIOS_TYPE1_SKUNumber比对
:CHKBIOS-ECVer rem 检查 BIOS 和 EC 版本信息
:ParsePreloadVer rem 从mes回复%SW_VER%中提取出%preloadver%
:CHKCPU rem 和mes比对的部分已经被注释掉
:CHKRAM rem 和mes比对内存大小
:CHKSSD rem 和mes比对SSD大小
:CHKME rem Check ME Lock state
:LPLoaddef rem 调用 InsydeH2OUVE\H2OUVE-W-CONSOLEx64.exe 工具，Load default setup language to EN-US
:BIOSLoadDefault rem 调用 HWOemWMIMethod.exe 工具，进行 BIOS 加载默认设置操作
:MOPS rem download shipping image, 调用 X:\PreloadGuide\MOPS.cmd 脚本，传入 AOD 路径、模块路径和日志文件路径参数；调用 W:\SWWORK\WinpeDoWork.cmd 脚本，传入 AOD 路径参数
:EXITSUCCESS rem wpeutil reboot 重启 WinPE 环境, 为什么reboot后还有后面的代码？
:createEFIPar rem 调用diskpart指令初始化S盘, 拷贝BOOT_MOPS(USB启动文件)到S盘
:BootOrder_HDD_USB_PXE rem BootFromPE，使用bcdedit.exe工具将机台设定为从PE启动
:Check_BIOS_Exist rem 检查 BIOS 文件是否存在
:Download_BIOS rem 下载 BIOS 文件, 实际服务器上的BIOS文件夹为空？
:startFlashBIOS rem start /w %bios_file% ，启动刷 BIOS 程序并等待其执行完成，后续逻辑有问题？
:LockME rem fptw64.exe -closemnf -Y>LOCKME.TXT，调用 fptw64.exe 工具，Lock ME --> wpeutil reboot，重启 WinPE 环境
```
```batch
:BootOrder_HDD_USB_PXE
HWOemWMIMethod.exe 5 1 50    >> %LOGPATH%  REM 调用 HWOemWMIMethod.exe 工具，检查可引导设备信息，将结果追加到日志文件中
find /i "index[00]:00 index[01]:00" %LOGPATH%  REM 在日志文件中查找 "index[00]:00 index[01]:00"，以便Check Bootable Device
bcdedit /enum {default}>bcddump.txt  REM 调用 bcdedit 工具，枚举默认启动项，将结果保存到 bcddump.txt 文件中
find /i "{ramdiskoptions}" bcddump.txt  REM 在 bcddump.txt 文件中查找 "{ramdiskoptions}"，以便entry in BCD,no need to create WinPE again
rem 确认服务器和本地存不存在BOOT_MOPS文件夹，以便重新拷贝，或者goto fail
REM 将boot.sdi，BootFromPE.cmd ，boot.wim复制到 c:\BOOT_MOPS\ 目录
call c:\BOOT_MOPS\BootFromPE.cmd %LOGPATH%  REM 调用 c:\BOOT_MOPS\BootFromPE.cmd 脚本，传入日志文件路径参数
```

#### call X:\PreloadGuide\MOPS.cmd %AODPATH% %MODULEPATH% %LOGPATH%
```batch
:ImageWhiteListCheck rem 使用ImageWhitelist.exe读取BIOS中的机种名，并和AODSTAT.DAT读取的机种名进行比对，如果不一致则fail
:AODMD5Verify rem MD5Verify.exe -f AODSTAT.DAT -c AODSTAT.md5，确认AODSTAT.DAT文件的MD5值和AODSTAT.md5中的值一致
:MD5Verify rem MD5Verify.exe -f %BETAMODULESPATH%\!modulename!.zip -v %%j，确认每个module文件的MD5值是否和AODSTAT.DAT中的一致，并将模块名称写入对应的 wt_modules_%%a.txt 文件，其中%%a对应各个module的module type，通常为Filter/0/1/2/3/4
:RUNMOPS rem call :ParseCRI Filter 
:Start rem call :ParseCRI %Type%，其中%Type%为0~4循环执行
:ParseCRI rem 获取module对应CRI文件中的%stdout%，执行解压和setup.cmd
rem stdout=7z.exe x j:\FBPartitionCheck02CN.zip -y -pWingtech -ox:\
rem stdout=x:\DiskFormatCheck\setup.cmd %AODPATH%
```

#### call W:\SWWORK\WinpeDoWork.cmd %AODPATH% %AODPATH%？

#### x:\DiskFormatCheck\setup.cmd %AODPATH%？

### OA3 process

### MES交互逻辑&卡控