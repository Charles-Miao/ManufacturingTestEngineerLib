```MERMAID
graph TD
    A[Start] --> B[显示CPU检查标题]
    B --> C[清理日志文件]
    C --> D[获取CPU信息到CPU.log]
    D --> E[预处理CPU变量]
    E --> F{CPU名称匹配检查}
    
    F --> |匹配| G[设置CHKCPU=PASS]
    F --> |不匹配| H[记录CPU错误]
    H --> I[生成对比日志]
    I --> J[显示错误信息]
    J --> K[暂停并重新检测]
    K --> A

    style G fill:#cfc,stroke:#090
    style H fill:#f9f,stroke:#333

    subgraph 预处理流程
        E1[原始CPU变量] --> E2[替换下划线为空格]
    end
    
    subgraph 错误日志生成
        H1[写入错误头信息] --> H2[记录BOM配置值]
        H2 --> H3[附加实际CPU信息]
        H3 --> H4[过滤WMIC标题行]
    end