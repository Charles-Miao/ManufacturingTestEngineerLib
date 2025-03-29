```Mermaid
flowchart TD
    Start --> A[Delete CFGCHK.log & SD.log]
    A --> B[Run devcon.exe find USB*]
    B --> C{Check for\nVID_0BDA&PID_0129}
    C -->|Found| PASS
    C -->|Not Found| D{Check for\nUSB2.0-CRW}
    D -->|Found| PASS
    D -->|Not Found| Fail
    
    Fail --> E[Call RED.CMD]
    E --> F[Extract SD Name]
    F --> G[Write to log]
    G --> H[Show Error]
    H --> I[Pause]
    I --> Start
    
    PASS --> J[Set CHKSD=PASS]
    J --> K[Timeout 1]