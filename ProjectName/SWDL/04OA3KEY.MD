```MERMAID
graph TD
    Start[Start] --> CheckBOM{检查BOM参数}
    CheckBOM -->|ProductkeyID/Productkey为空| BOMF[BOM参数错误处理]
    CheckBOM -->|参数有效| CreateOA3[生成OA3.bin]
    
    CreateOA3 -->|生成失败| OA3F[OA3生成失败处理]
    CreateOA3 --> WriteOA3[写入OA3.bin]
    
    WriteOA3 -->|写入失败| RetryWrite[重试写入]
    RetryWrite --> WriteOA3
    WriteOA3 -->|写入成功| GenerateCBR[生成CBR报告]
    
    GenerateCBR -->|报告生成失败| CBRF[CBR报告失败处理]
    GenerateCBR --> ValidateCBR{验证CBR}
    
    ValidateCBR -->|验证失败| CBRhwfail[CBR硬件哈希失败]
    ValidateCBR -->|验证通过| FinalPass[设置最终状态]
    
    BOMF --> AlertBOM[红灯警报/日志记录]
    OA3F --> AlertOA3[红灯警报/日志记录]
    CBRF --> AlertCBR[红灯警报/日志记录]
    CBRhwfail --> AlertCBRhw[红灯警报/日志记录]
    
    AlertBOM --> PauseBOM[暂停等待处理]
    AlertOA3 --> PauseOA3[暂停等待处理]
    AlertCBR --> PauseCBR[暂停等待处理]
    AlertCBRhw --> PauseCBRhw[暂停等待处理]
    
    PauseBOM -->|重试| BOMF
    PauseOA3 -->|重试| CreateOA3
    PauseCBR -->|重试| GenerateCBR
    PauseCBRhw -->|重试| GenerateCBR
    
    FinalPass --> SetStatus[设置ProductKeyState/HW_Hash]
    SetStatus --> PASS[设置OA3KEY=PASS]

    classDef process fill:#e6f3ff,stroke:#0066cc;
    classDef condition fill:#ffe6cc,stroke:#ff9900;
    classDef error fill:#ffd9cc,stroke:#ff3300;
    classDef success fill:#d9f2d9,stroke:#009933;
    
    class CheckBOM,ValidateCBR condition;
    class CreateOA3,WriteOA3,GenerateCBR,AlertBOM,AlertOA3,AlertCBR,AlertCBRhw process;
    class BOMF,OA3F,CBRF,CBRhwfail,PauseBOM,PauseOA3,PauseCBR,PauseCBRhw error;
    class FinalPass,PASS success;
```