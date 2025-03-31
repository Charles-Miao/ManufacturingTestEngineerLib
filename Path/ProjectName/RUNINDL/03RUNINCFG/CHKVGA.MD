```mermaid
graph TD
    A[Start] --> B[清理日志文件]
    B --> C[获取PCI设备列表到VGA.log]
    C --> D{检查VGATP类型}
    
    D --> |VGATP=UMA| E[直接通过验证]
    D --> |其他类型| F[构造设备ID]
    F --> G{设备存在检查}
    
    G --> |存在| E
    G --> |不存在| H[记录VGA错误]
    H --> I[显示错误信息]
    I --> J[暂停并重新检测]
    J --> A

    style E fill:#cfc,stroke:#090
    style H fill:#f9f,stroke:#333

    subgraph 设备ID构造流程
        F1[原始VGATP值] --> F2[截取前4位作为VEN]
        F2 --> F3[截取4-8位作为DEV]
        F3 --> F4[组合成VEN_XXXX&DEV_XXXX]
    end
    
    subgraph 特殊处理逻辑
        CheckUMA{是否为集成显卡，UMA Unified Memory Architecture 通常指代集成显卡方案} -->|是| Bypass[跳过设备检查]
        CheckUMA -->|否| NormalCheck[执行设备验证]
    end