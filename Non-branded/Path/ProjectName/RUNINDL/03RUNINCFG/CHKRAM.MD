```Mermaid
graph TD
    A[Start] --> B[初始化变量]
    B --> C[获取内存条容量]
    C --> D[清理数据空格]
    D --> E[计算总容量字节]
    E --> F[转换为GB]
    F --> G{容量比对}
    
    G --> |匹配| H[生成PASS文件]
    G --> |不匹配| I[记录错误日志]
    I --> J[显示错误信息]
    J --> K[重新检测]
    K --> A

    style H fill:#cfc,stroke:#090
    style I fill:#f9f,stroke:#333

    subgraph 内存数据处理流程
        C1[WMIC输出示例] --> C2["Capacity
        8589934592
        8589934592"]
        C2 --> C3[跳过标题行]
        C3 --> C4[存储到RAM1-RAM8]
    end

    subgraph 容量计算逻辑
        E1[单个内存条值] --> E2[截取前N-3字符]
        E2 --> E3[累加所有条数值]
        E3 --> E4[两次/1024换算]
        E4 --> E5[得到GB单位值]
    end