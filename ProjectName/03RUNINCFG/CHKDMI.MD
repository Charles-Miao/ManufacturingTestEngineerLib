```mermaid
graph TD
    A[Start] --> B[显示标题信息]
    B --> C[删除CFGCHK.log]
    C --> D[调用BOM.bat]
    D --> E[1秒等待]
    E --> F{SN检查}
    
    F --> |成功| G{UUID检查}
    F --> |失败| H[记录SN错误]
    H --> I[显示错误]
    I --> J[暂停]
    J --> A
    
    G --> |成功| K{System Family检查}
    G --> |失败| L[记录UUID错误]
    L --> M[显示错误]
    M --> N[暂停]
    N --> A
    
    K --> |成功| O{System Product检查}
    K --> |失败| P[记录SF错误]
    P --> Q[显示错误]
    Q --> R[暂停]
    R --> A
    
    O --> |成功| S[设置CHKDMI=PASS]
    O --> |失败| T[记录SP错误]
    T --> U[显示错误]
    U --> V[暂停]
    V --> A