```Mermaid
flowchart TD
    Start --> A[Display Header]
    A --> B[Timeout 1]
    B --> C[Call BOM.bat]
    C --> D[Copy Driver Files]
    D --> E[Load mtkwl6ex.inf]
    E --> F[Timeout 5]
    F --> G[Load Netwtw08.INF]
    G --> H[Timeout 5]
    H --> I[Delete Log Files]
    I --> J[Find PCI Devices]
    J --> K{Check WLAN\nin Log?}
    K -->|Found| PASS
    K -->|Not Found| WLANFail
    
    WLANFail --> L[Call RED.CMD]
    L --> M[Extract WLAN1]
    M --> N[Write Log Entries]
    N --> O[Show Error]
    O --> P[Pause]
    P --> Start
    
    PASS --> Q[Set CHKWLAN=PASS]
    Q --> R[Timeout 1]