```MERMAID
graph TD
    A[Start] --> B[显示摄像头检查标题]
    B --> C[清理日志文件]
    C --> D[获取USB设备列表到CAM.log]
    D --> E{解析%Camera%变量}
    
    E --> F[检查VID_%%a&PID_%%b]
    F --> |存在| G[设置CHKCAM=PASS]
    E --> H[检查VID_%%c&PID_%%d]
    H --> |存在| G
    
    F --> |不存在| I[检查下一个组合]
    H --> |不存在| J[所有组合检查完毕?]
    J --> |是| K[记录摄像头错误]
    K --> L[提取当前摄像头信息]
    L --> M[生成错误日志]
    M --> N[显示错误信息]
    N --> O[暂停并重新检测]
    O --> A

    style G fill:#cfc,stroke:#090
    style K fill:#f9f,stroke:#333