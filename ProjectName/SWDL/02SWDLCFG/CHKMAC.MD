```MERMAID
graph TD
    Start[Start] --> Init[初始化]
    Init --> CallBOM[调用BOM.bat]
    CallBOM --> CleanLog[删除旧日志]
    CleanLog --> CheckMACs{检查MAC地址}
    
    CheckMACs -->|任一MAC为空| MACFail[MAC错误处理]
    CheckMACs -->|全部存在| PASS[设置PASS标志]
    
    MACFail --> RedAlert[切换红灯/RED.CMD]
    RedAlert --> LogError[记录错误日志]
    LogError --> ShowErr[显示错误信息]
    ShowErr --> Pause[用户关注的pause命令]
    Pause --> Restart[重新执行检测]
    Restart --> Start
    
    classDef startend fill:#d9f2d9,stroke:#009933;
    classDef process fill:#e6f3ff,stroke:#0066cc;
    classDef condition fill:#ffe6cc,stroke:#ff9900;
    classDef error fill:#ffd9cc,stroke:#ff3300;
    
    class Start,PASS startend;
    class Init,CallBOM,CleanLog,RedAlert,LogError,ShowErr process;
    class CheckMACs condition;
    class MACFail,Pause,Restart error;