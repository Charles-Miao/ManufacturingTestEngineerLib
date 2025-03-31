- Set Boot Order Normal

```mermaid
flowchart TD
    Start --> A[Initialize normal=0]
    A --> B[Set Console Color]
    B --> C[Display Header]
    C --> D[Write 0xDD to 0x4F1]
    D --> E[Delay 2s]
    E --> F[Read Register Value]
    F --> G{Value == DD?}
    G -->|Yes| H[Delay 2s]
    G -->|No| I[Increment normal]
    
    I --> J{Attempts >=5?}
    J -->|No| Start
    J -->|Yes| K[Error Display Loop]
    
    H --> End
    K -->|Loop| K
    
    style Start fill:#9f9,color:black
    style End fill:#7f7,color:white
    style K fill:#f44,color:white