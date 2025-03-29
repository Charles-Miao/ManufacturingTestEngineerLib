```mermaid
graph TD
    A[Start] --> B[初始化环境]
    B --> C[获取硬盘信息]
    C --> D{SSD1型号验证}
    
    D --> |匹配HDD1TP| E{SSD1容量验证}
    D --> |不匹配| F[尝试替换_为空格]
    F --> |匹配| E
    F --> |仍不匹配| G[HDD失败]
    
    E --> |容量匹配| H{SSD2检查}
    E --> |容量不匹配| G
    
    H --> |无SSD2配置| I[通过验证]
    H --> |有SSD2配置| J{SSD2型号验证}
    J --> |匹配| K{SSD2容量验证}
    J --> |不匹配| G
    
    K --> |容量匹配| I
    K --> |容量不匹配| G
    
    G --> L[记录错误日志]
    L --> M[显示错误信息]
    M --> N[重新检测]
    N --> A

    style I fill:#cfc,stroke:#090
    style G fill:#f9f,stroke:#333