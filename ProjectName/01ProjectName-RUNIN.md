```mermaid
graph TD
    Start[Start] --> LEDSetup[设置LED为黄色]
    LEDSetup --> CopyTools[复制工具和批处理文件]
    CopyTools --> CheckRUNINCFG{存在RUNINCFG.bat?}
    CheckRUNINCFG -- 否 --> CopyTools
    CheckRUNINCFG -- 是 --> SetBootToLan[设置启动到网络]
    
    SetBootToLan --> CallSetBoot[调用SetBootToLan.bat]
    CallSetBoot --> CheckSetBoot{成功?}
    CheckSetBoot -- FAIL --> SetBootToLan
    CheckSetBoot -- 成功 --> TimeSync[同步时间]
    
    TimeSync --> CopyTimeTools[复制时间同步工具]
    CopyTimeTools --> CheckTimeBat{存在TimeSet.bat?}
    CheckTimeBat -- 否 --> TimeSync
    CheckTimeBat -- 是 --> CallTimeSet[调用TimeSet.bat]
    CallTimeSet --> CheckTimeResult{成功?}
    CheckTimeResult -- FAIL --> TimeSync
    CheckTimeResult -- 成功 --> CFGCheck[配置检查]
    
    CFGCheck --> CallCFG[调用RUNINCFG.bat]
    CallCFG --> CheckCFGResult{成功?}
    CheckCFGResult -- FAIL --> CFGCheck
    CheckCFGResult -- 成功 --> DownloadImage[下载镜像]
    
    DownloadImage --> CallYELLOW[调用YELLOW.CMD]
    CallYELLOW --> CopyRUNINDL[复制RUNINDL.bat]
    CopyRUNINDL --> InitFlag[设置IMGiTFLG=FAIL]
    InitFlag --> CallRUNINDL[调用RUNINDL.bat]
    CallRUNINDL --> OutputFlag[输出IMGiTFLG]
    
    OutputFlag --> End[结束流程]
    End --> Reboot[等待30秒后重启]

    subgraph RUNINDL内部流程
    CallRUNINDL -->|进入RUNINDL.bat| DisableFW[禁用防火墙]
    DisableFW --> SetVar[设置变量]
    SetVar --> CopyIMGit[复制IMGiT64文件]
    CopyIMGit --> CheckRestoreCmd{存在Restore.cmd?}
    CheckRestoreCmd -- 否 --> CopyIMGit
    CheckRestoreCmd -- 是 --> RestoreImage[恢复镜像]
    
    RestoreImage --> CheckImageResult{IMGiTFLG=PASS?}
    CheckImageResult -- 否 --> FailHandle[显示失败警告并重启流程]
    FailHandle --> Start
    CheckImageResult -- 是 --> Copy7z[复制7z文件]
    Copy7z --> CopyBOM[复制BOM.bat]
    CopyBOM --> LoadBIOS[加载BIOS设置]
    LoadBIOS --> BootNormal[启动正常模式]
    BootNormal --> LogRecord[记录日志]
    end

    style Start fill:#90EE90
    style End fill:#FFB6C1
    style FailHandle fill:#FFCCCB
    style SetBootToLan stroke:#FFD700
    style TimeSync stroke:#FFD700
    style CFGCheck stroke:#FFD700
    style DownloadImage stroke:#FFD700