```mermaid
graph TD
    Start[Start] --> A[显示工作站]
    A --> B[终止Route.exe进程]
    B --> C[启动Route.exe SWDL]
    C --> D[检查网卡速度]
    
    D -->|NicSpeed=10/100| E[设置LANSPEED并跳转LAN_FAIL]
    D -->|正常速度| F[平台主流程]
    
    E --> G[显示红色警告]
    G --> H[暂停并返回速度检查]
    H --> D
    
    F --> I{检查SWDL.BAT是否存在?}
    I -->|存在| J[复制并执行SWDL.BAT]
    J --> K[End]
    I -->|不存在| L[显示红色错误]
    L --> M[暂停并尝试重新检查]
    M --> N[跳转CK_MODID（未定义）]
    
    style Start fill:#9f9,stroke:#333
    style E fill:#f96,stroke:#333
    style L fill:#f96,stroke:#333
    style N stroke:#f00,stroke-width:2px