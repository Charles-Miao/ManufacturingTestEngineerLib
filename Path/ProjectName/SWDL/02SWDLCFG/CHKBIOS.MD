```MERMAID
graph TD
    Start((开始)) --> GetInfo[获取BIOS/EC版本信息]
    GetInfo --> CheckBIOS{BIOS版本校验}
    
    CheckBIOS -->|版本匹配| CheckEC{EC版本校验}
    CheckBIOS -->|版本不匹配| BIOSError[BIOS错误处理]
    
    CheckEC -->|版本匹配| Success[设置成功标志]
    CheckEC -->|版本不匹配| ECError[EC错误处理]
    
    BIOSError --> Log1[生成错误日志]
    ECError --> Log2[生成错误日志]
    
    Log1 --> Restart1[重新开始流程]
    Log2 --> Restart2[重新开始流程]
    
    Restart1 --> GetInfo
    Restart2 --> GetInfo
    
    Success --> End((结束))

    classDef startend fill:#4CAF50,stroke:#388E3C;
    classDef process fill:#2196F3,stroke:#1976D2;
    classDef decision fill:#FF9800,stroke:#F57C00;
    classDef error fill:#f44336,stroke:#d32f2f;
    class Start,End startend;
    class CheckBIOS,CheckEC decision;
    class BIOSError,ECError,Log1,Log2,Restart1,Restart2 error;
    classDef default process;