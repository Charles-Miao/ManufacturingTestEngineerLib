```MERMAID
graph TD
    Start((开始)) --> Init[初始化防火墙设置]
    Init --> CheckImg{镜像名称校验}
    
    CheckImg -->|名称有效| Copy[复制镜像工具]
    CheckImg -->|名称无效| ImgError[镜像名称错误]
    
    Copy --> Restore[执行镜像恢复]
    Restore -->|恢复成功| Load[set BIOS load default]
    Restore -->|恢复失败| ResError[恢复失败错误]
    
    Load --> Boot[Set Boot Order Normal]
    Boot --> Log[备份操作日志]
    
    ImgError -->|重试| CheckImg
    ResError -->|重试| CheckImg

    classDef startend fill:#4CAF50,stroke:#388E3C;
    classDef process fill:#2196F3,stroke:#1976D2;
    classDef decision fill:#FF9800,stroke:#F57C00;
    classDef error fill:#f44336,stroke:#d32f2f;
    class Start,Log startend;
    class CheckImg decision;
    class ImgError,ResError error;
    classDef default process;