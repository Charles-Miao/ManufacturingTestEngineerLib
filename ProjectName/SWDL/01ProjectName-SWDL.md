```mermaid
graph TD
    Start[Start] --> CopyLED[复制LED控制工具]
    CopyLED --> CallLED[调用LED控制脚本]
    CallLED --> CopyFiles[复制工具和批处理文件]
    CopyFiles -->|文件不存在| CopyFiles
    CopyFiles --> SetBoot[设置网络启动]
    SetBoot -->|失败| SetBoot
    SetBoot --> TimeSync[时间同步]
    TimeSync -->|失败| TimeSync
    TimeSync --> CFGCheck[配置检查]
    CFGCheck -->|失败| CFGCheck
    CFGCheck --> Download[下载镜像]
    Download -->|失败| Download
    Download --> OA3Key[OA3密钥处理]
    OA3Key -->|DPK标记=N| Shipping[出厂设置]
    OA3Key -->|失败| OA3Key
    Shipping --> Report[报告成功状态]
    Report -->|服务器响应异常| Report
    Report --> ShowPass[显示PASS信息]
    ShowPass --> Shutdown[关机流程]
    
    classDef process fill:#e6f3ff,stroke:#0066cc;
    classDef condition fill:#ffe6cc,stroke:#ff9900;
    classDef terminal fill:#d9f2d9,stroke:#009933;
    
    class Start,ShowPass,Shutdown terminal;
    class CopyLED,CallLED,CopyFiles,SetBoot,TimeSync,CFGCheck,Download,OA3Key,Shipping,Report process;