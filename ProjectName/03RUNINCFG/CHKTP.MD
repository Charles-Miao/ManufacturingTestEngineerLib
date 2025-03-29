```Mermaid
flowchart TD
    Start --> A[Delete CFGCHK.log & TP.log]
    A --> B[Run devcon.exe find HID*]
    B --> C{Check for\nTouch Pad}
    C -->|Found| PASS
    C -->|Not Found| Fail
    
    Fail --> D[Call RED.CMD]
    D --> E[Write Error to log]
    E --> F[Show Error]
    F --> G[Pause]
    G --> Start
    
    PASS --> H[Set CHKTP=PASS]
    H --> I[Timeout 1]