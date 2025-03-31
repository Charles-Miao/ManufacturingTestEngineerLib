```MERMAID
flowchart TD
    Start --> A[Initialize Speed Levels]
    A --> B{Model Detection}
    B -->|ARN39P| C[Set ARN39P Parameters]
    B -->|MTN39C| D[Set MTN39C Parameters]
    B -->|DN39-140H2| E[Set DN39-140H2 Parameters]
    B -->|Others| F[NewModel Handler]
    
    C --> G[FANL Test]
    D --> G
    E --> G
    
    G --> H[Low Speed Test]
    H --> I{Check FAN1}
    I -->|Valid| J{Check FAN2}
    I -->|Invalid| FAIL
    J -->|Valid| K[Medium Speed Test]
    J -->|Invalid| FAIL
    
    K --> L{Check FAN1}
    L -->|Valid| M{Check FAN2}
    L -->|Invalid| FAIL
    M -->|Valid| N[High Speed Test]
    M -->|Invalid| FAIL
    
    N --> O{Check FAN1}
    O -->|Valid| P{Check FAN2}
    O -->|Invalid| FAIL
    P -->|Valid| PASS
    P -->|Invalid| FAIL
    
    FAIL --> Q{Retry?}
    Q -->|Yes| Start
    Q -->|No| R[Error Display]
    R --> Start
    
    PASS --> S[Reset Fan Control]
    
    F --> T[New Model Alert]
    T --> Start