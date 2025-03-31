- 获取MAC，rdMacID.exe USB %IP%>MAC.cmd
- 获取对应线别的rdSwPort.ini，ipconfig /all | find "IPv4 Address. . . . . . . . . . . : 10.101" && copy rdSwPort101.ini rdSwPort.ini /y
- 获取MAC对应的Port，rdSwPort.exe %MACID%> GetPort.cmd
- 获取网络速度，getnicspeed.exe
- 除了黄色之外，都删除del GetPort_*.cmd
- 设定UpdateLEDPortStatus.ini
- 更新灯的颜色，UpdateLEDPortStatus.exe %color% %2（port，颜色，站别等）

```mermaid
flowchart TD
    Start[":Start"] --> Init["调用BOM.bat\n设置参数"]
    Init -->|设置超时| SetParam["Interval=%3\nSTEP=2\ncolor=%1"]
    SetParam --> GetIP["获取IP地址\nIPCONFIG /ALL > IP.log"]
    
    GetIP --> ParseIP["解析IP地址\nfor/f命令提取IPv4"]
    ParseIP --> GetMAC["获取MAC地址\nrdMacID.exe USB/Realtek"]
    
    GetMAC --> CheckMAC{"MACID有效？"}
    CheckMAC -- NONE --> End[":END"]
    CheckMAC -- 有效 --> InitSwitch[":INI"]
    
    InitSwitch --> LoopINI["循环配置交换机端口文件\n(最多5次重试)"]
    LoopINI --> CheckINI{"rdSwPort.ini\n存在？"}
    CheckINI -- 不存在 --> LoopINI
    CheckINI -- 存在 --> GetCorePort["获取CorePort\ncall GetPort_%SN%.cmd"]
    
    GetCorePort --> CheckPort{"CorePort\n有效？"}
    CheckPort -- 空 --> LoopGetPort[":GetPort\n(最多3次重试)"]
    LoopGetPort --> CheckPort
    CheckPort -- 有效 --> LanSpeed[":LANSP"]
    
    LanSpeed --> GetSpeed["获取网络速度\ngetnicspeed.exe"]
    GetSpeed --> CleanFiles["清理临时文件\n按颜色参数删除"]
    CleanFiles --> GenConfig["生成配置文件\nUpdateLEDPortStatus.ini"]
    
    GenConfig --> Upload["上传状态\nUpdateLEDPortStatus.exe"]
    Upload --> End
    
    style Start fill:#f9f,stroke:#333
    style End fill:#f9f,stroke:#333
    style LoopINI fill:#ffd,stroke:#d90
    style LoopGetPort fill:#ffd,stroke:#d90
    style GetSpeed fill:#e6f3ff,stroke:#4da3ff
    style CleanFiles fill:#ffe6e6,stroke:#ff4d4d