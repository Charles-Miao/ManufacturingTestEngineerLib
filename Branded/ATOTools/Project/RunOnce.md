```mermaid
graph TD
    A[开始] --> B[设置日志路径LOGPATH]
    B --> C{检查Install_Update.cmd是否存在?}
    C -- 是 --> D[执行Install_Update.cmd]
    C -- 否 --> E[删除SSD驱动]
    E --> F[禁用Windows Update任务计划]
    F --> G[添加启动项GetTools.cmd]
    G --> H{存在WallpaperSet.cmd?}
    H -- 是 --> I[执行壁纸设置脚本]
    H -- 否 --> J[执行WTIO设备操作]
    J --> K[获取磁盘不安全关机计数]
    K --> L{UnsafeShutdowns ≥80?}
    L -- 是 --> M[显示错误界面并暂停]
    M --> L
    L -- 否 --> N[保存控制台日志]
    N --> O[20秒后重启系统]
    O --> P[结束]
    
    style A fill:#4CAF50,stroke:#388E3C
    style P fill:#F44336,stroke:#D32F2F
    classDef condition fill:#FFC107,stroke:#FFA000;
    class C,H,L condition