- REM set 0x4F2 to 0xAA,means set BIOS load default

```MERMAID
flowchart TD
    Start --> A[Set Console Color]
    A --> B[Display Header]
    B --> C[Write 0xAA to 0x4F2]
    C --> D[Delay 2s]
    D --> E[Read 0x4F2 Value]
    E --> F{Value == AA?}
    F -->|Yes| G[Delay 1s]
    F -->|No| Start
    G --> End
    
    style Start fill:#9f9,color:black
    style End fill:#f99,color:white