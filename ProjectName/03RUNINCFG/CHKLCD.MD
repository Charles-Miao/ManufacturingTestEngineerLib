```MERMAID
graph TD
    A[Start] --> B[显示LCD检查标题]
    B --> C[清理日志文件]
    C --> D[获取显示器设备列表]
    D --> E[提取当前LCD名称]
    
    E --> F{精确匹配检查}
    F --> |变量相同| G[设置CHKLCD=PASS]
    F --> |不匹配| H{模糊匹配检查}
    
    H --> |成功| G
    H --> |失败| I[记录LCD错误]
    I --> J[生成对比日志]
    J --> K[显示错误信息]
    K --> L[暂停并重新检测]
    L --> A

    style G fill:#cfc,stroke:#090
    style I fill:#f9f,stroke:#333