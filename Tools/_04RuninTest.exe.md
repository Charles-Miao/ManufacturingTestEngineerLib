Runin测试
##

Reboot（重启测试）
###

- 原理：通过循环冷/热启动验证硬件初始化稳定性
- 冷启动（Cold Boot）：完全断电后重新上电启动，触发完整POST自检
- 热启动（Warm Boot）：通过复位信号保持通电状态的重启（如Ctrl+Alt+Del）
- 差异点：冷启动会重置CMOS时钟/初始化所有总线设备，热启动保留内存数据
- 用途：检测电源管理模块、主板电路、系统引导异常
- 覆盖异常：启动失败、BIOS固件错误、硬件初始化失败

Burnin综合压力测试(CPU Test/ 2D Graphics Test/ Disk/ Sound Test/ Memory Test/ Video Playback Test)
###
- CPU Test：满负荷运算（如Prime95算法）
- 2D Graphics：高分辨率位图渲染测试
- Disk：持续读写IO压力测试（坏道检测）
- Sound Test：音频信号全频段输出检测
- Memory Test：内存填充/校验循环（MemTest86原理）
- Video Playback：视频解码硬件加速测试 
- 覆盖异常：组件过热、负载稳定性、接口通信错误

Sleep（睡眠唤醒）
###
- 原理：验证ACPI S3状态电源管理 
- 用途：检测内存供电保持能力、唤醒信号电路 
- 覆盖异常：唤醒黑屏、设备掉电、外设失联

3DMark（图形性能）
###
- 原理：DirectX/OpenGL基准测试套件 
- 用途：GPU散热能力验证、显存稳定性测试 
- 覆盖异常：画面撕裂、驱动超时、显存溢出

S4（休眠测试）
###
- 原理：系统完整状态写入硬盘的深度休眠 
- 用途：验证硬盘完整性、休眠文件系统兼容性 
- 覆盖异常：恢复数据丢失、启动卷损坏

AIDA64稳定性测试(System Stability Test)
###
- 原理：多组件联合压力测试（FPU/Cache/RAM） 
- 用途：检测供电电路波动、散热系统效能 
- 覆盖异常：电压不稳、散热失效、硬件节流

Video（视频测试）
###

- 原理：多种编码格式硬解码测试（H.264/HEVC/AV1） 
- 用途：验证媒体引擎、显示输出接口稳定性 
- 覆盖异常：解码错误、色深异常、HDMI/DP信号中断

​
整体目标
###
- 通过持续负载暴露早期故障（INFANT MORTALITY），覆盖生产装配中的焊接不良、散热器装配问题、元件批次缺陷等潜在异常。

Runin Dashboard
##

1. PC根据IP获取MAC，再根据MAC获取Port（此Port和交换机口的MAC一一对应），再获取LAN speed，最后使用厂商提供的工具将机台的信息（SN/ Step[OnRack:1 or Others:2]/ Internal[间隔时间]/ modelname/ Line/ PN/ WO/ Color/ 站别/ Switch IP/ Port/ Speed/ Item等）抛送给Database（此DB由厂商架设）
2. DataBase安装于10.100.20.8服务器，此DB由厂商配置和安装
3. RackAgent.exe由厂商开发，主要有两个功能（由RACK_SHOW_STATUS参数控制）：第一个功能是用于产线展示Runin的运行状况，另一个用于控制Runin架子的灯号（白色，红色，黄色，绿色，蓝色，紫色），控制灯号的程式开在10.X.1.5服务器上
4. Runin架子上，每个位置都有一个灯，用于展示机台的运行状况（红色，黄色，绿色等）
5. PC将上架的时间抛送给MES，RackAgent.exe会透过MES获取上架时间
6. RackAgent.exe除了显示灯号之外，右击每个框格，会显示机台SN，IP，状态，上架时间，状态改变时间，老化总时间，测试项目，网络速度等信息

```mermaid
graph TD
    subgraph PC端
        PC["PC终端"] -->|1.根据IP获取MAC| GetMAC["获取MAC地址"]
        GetMAC -->|2.根据MAC获取Port| GetPort["获取交换机端口"]
        GetPort -->|3.获取LAN Speed| GetSpeed["获取网络速度"]
        GetSpeed -->|4.发送设备信息| Database[("Database\n10.100.20.8")]
    end

    subgraph 服务端
        Database -->|存储数据| RackAgent_Server["RackAgent.exe\n(服务端)"]
        RackAgent_Server -->|功能分支| StatusShow["产线状态展示"]
        RackAgent_Server -->|功能分支| LightControl["灯号控制程序\n10.X.1.5"]
    end

    subgraph 产线端
        RackAgent_Line["RackAgent.exe\n(产线端)"] -->|从MES获取| MES[["MES系统"]]
        LightControl -->|控制信号| RuninRack["Runin测试架"]
        RuninRack -->|状态反馈| LightStatus["状态灯\n(红/黄/绿等)"]
    end

    PC -->|5.上架时间| MES
    RackAgent_Line -->|6.右键查看详情| DetailWindow["详细信息窗口\nSN/IP/状态时间/测试项等"]

    style Database fill:#f9f,stroke:#333
    style RuninRack fill:#cff,stroke:#333
    style LightControl fill:#ff9,stroke:#333