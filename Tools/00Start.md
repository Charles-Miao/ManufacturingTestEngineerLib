```mermaid
graph TD
    A[Start] --> B[Change Directory to TOOLS]
    B --> C[Wait 3 seconds]
    C --> D[Call BOM.bat]
    D --> E{BOM.bat exists in C:\AFT?}
    E -->|Yes| F[Goto GetBOM]
    E -->|No| G[Run AMIDEWINx64.EXE /pat]
    G --> H{Output contains RUNPASS?}
    H -->|Yes| F
    H -->|No| I{SN exists in C:\RUNING.log?}
    I -->|Yes| J[Goto RUNIN]
    I -->|No| K[Wait 30 seconds]
    K --> L{PETEST*.7Z exists?}
    L -->|Yes| M[Extract PETEST*.7Z]
    M --> N[Wait 3 seconds]
    N --> O[Call Runin.BAT]
    O --> J
    L -->|No| P[7z file not found]
    P --> A
    J --> Q[Log SN to C:\RUNING.log]
    Q --> R[Wait 60 seconds]
    R --> S{RuninTest.exe is running?}
    S -->|Yes| J
    S -->|No| T[Start RuninTest.exe]
    T --> J
    F --> U[Delete BOM.BAT and result.txt]
    U --> V[Call getbom.bat]
    V --> W{BOM.BAT exists?}
    W -->|Yes| X[Check StartTime]
    X -->|StartTime is empty| Y[Call getbomST.bat]
    X -->|StartTime is not empty| Z[Goto CheckStation]
    Y --> Z
    W -->|No| U
    Z --> AA{Find SET CRT_STATION in BOM.bat}
    AA -->|Runin| AB[Goto AFT]
    AA -->|AFT| AB
    AA -->|Repair| AB
    AA -->|Not found| BB[Process error]
    BB --> CC[Pause]
    CC --> Z
    AB --> AC[Call Patch.bat]
    AC --> AD[Wait 1 second]
    AD --> AE[Call AFT.BAT]
    AE --> A
