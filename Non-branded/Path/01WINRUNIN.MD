```MERMAID
graph TD
    Start[Start] --> A[关闭防火墙]
    A --> B[停止防火墙服务]
    B --> C[显示工作站]
    C --> D[终止Route.exe进程]
    D --> E[启动Route.exe RUNIN]
    E --> F[检查网卡速度]
    
    F -->|NicSpeed=10/100| G[设置LANSPEED并跳转LAN_FAIL]
    F -->|正常速度| H[平台主流程]
    
    G --> I[显示红色警告]
    I --> J[暂停并返回速度检查]
    J --> F
    
    H --> K{检查RUNIN.BAT是否存在?}
    K -->|存在| L[复制并执行RUNIN.BAT]
    L --> H
    K -->|不存在| M[显示红色错误]
    M --> N[暂停并返回模型检查]
    N --> H
    
    style Start fill:#9f9,stroke:#333
    style G fill:#f96,stroke:#333
    style M fill:#f96,stroke:#333