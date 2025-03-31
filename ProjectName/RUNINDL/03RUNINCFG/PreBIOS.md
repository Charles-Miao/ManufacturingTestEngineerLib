```mermaid
graph TD
    A[Start] --> B[显示BIOS更新标题]
    B --> C[初始化环境]
    C --> D[获取当前BIOS/EC版本]
    D --> E{BIOS版本检查}
    
    E --> |匹配预设值| F{EC版本检查}
    E --> |不匹配| G[执行BIOS更新]
    G --> H[运行CDP.BAT，格式化硬盘]
    H --> I[运行AfuUpdate.bat]
    I --> J[暂停]
    J --> A
    
    F --> |匹配预设值| K[设置CHKBIOS=PASS]
    F --> |不匹配| L[记录EC错误]
    L --> M[显示错误]
    M --> N[暂停]
    N --> A
    
    style G fill:#f9f,stroke:#333,stroke-width:2px
    style K fill:#cfc,stroke:#090