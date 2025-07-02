```mermaid
graph TD
    A[START] --> B[显示流程标题]
    B --> C[执行GetRuninBom.bat]
    C --> D{检查CRT_STATION类型}
    
    D -->|FCOS1/Runin| E[Runin流程]
    D -->|AFT/SWDL/OBE| F[SWDL流程]
    D -->|其他值| G[路由错误处理]
    
    E --> H[复制LEDControlR]
    H --> I[调用LEDCtrl.bat]
    I --> J{检查StartTime}
    J -->|未设置| K[调用GetRuninBomST.bat]
    J -->|已设置| L[检查BOM.bat]
    L -->|包含From:Runin| M[执行RUNINP]
    L -->|消息非OK| N[消息失败处理]
    
    F --> O[复制LEDControlS]
    O --> P[调用LEDCtrl.bat]
    P --> Q[调用GetSWDLBom.bat]
    Q --> R[执行BOM.bat]
    R --> S{检查StartTime}
    S -->|未设置| T[调用GetSWDLBomST.bat]
    S -->|已设置| U[检查BOM.bat]
    U -->|包含From:SWDL| V[执行SWDLP]
    U -->|消息非OK| N
    
    M --> W[设置错误消息]
    W --> X[执行WINRUNIN.BAT]
    V --> Y[设置错误消息]
    Y --> Z[执行WINSWDL.BAT]
    
    X & Z --> AA[SetFail处理]
    G --> AB[LED红色提示]
    AB --> AC[显示路由错误]
    AC --> AD[调用HFail.bat]
    AD --> A
    
    AA --> AE[红色提示]
    AE --> AF[显示错误消息]
    AF --> A
    
    N --> AG[LED红色提示]
    AG --> AH[显示消息错误]
    AH --> AI[调用HFail.bat]
    AI --> A