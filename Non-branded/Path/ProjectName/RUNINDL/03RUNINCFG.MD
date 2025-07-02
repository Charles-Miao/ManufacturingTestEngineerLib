```Mermaid
graph TD
    Start[Start] --> Banner[显示启动横幅]
    Banner --> SETDMI[写入SSN/DMI信息]
    
    SETDMI --> CheckDMI{SETDMI==PASS?}
    CheckDMI -- 否 --> SETDMI
    CheckDMI -- 是 --> CHKBATT[检查电池]
    
    CHKBATT --> CheckBatt{CHKBATT==PASS?}
    CheckBatt -- 否 --> CHKBATT
    CheckBatt -- 是 --> PreBIOS[检查BIOS版本]
    
    PreBIOS --> CheckBIOS{CHKBIOS==PASS?}
    CheckBIOS -- 否 --> PreBIOS
    CheckBIOS -- 是 --> CHKBT[检查蓝牙]
    
    CHKBT --> CheckBT{CHKBT==PASS?}
    CheckBT -- 否 --> CHKBT
    CheckBT -- 是 --> CHKCAM[检查摄像头]
    
    CHKCAM --> CheckCAM{CHKCAM==PASS?}
    CheckCAM -- 否 --> CHKCAM
    CheckCAM -- 是 --> CHKCPU[检查CPU]
    
    CHKCPU --> CheckCPU{CHKCPU==PASS?}
    CheckCPU -- 否 --> CHKCPU
    CheckCPU -- 是 --> CHKVGA[检查显卡]
    
    CHKVGA --> CheckVGA{CHKVGA==PASS?}
    CheckVGA -- 否 --> CHKVGA
    CheckVGA -- 是 --> CHKHDD[检查硬盘]
    
    CHKHDD --> CheckHDD{日志含PASS?}
    CheckHDD -- 否 --> CHKHDD
    CheckHDD -- 是 --> CHKLAN[检查网卡]
    
    CHKLAN --> CheckLAN{CHKLAN==PASS?}
    CheckLAN -- 否 --> CHKLAN
    CheckLAN -- 是 --> CHKLCD[检查屏幕]
    
    CHKLCD --> CheckLCD{CHKLCD==PASS?}
    CheckLCD -- 否 --> CHKLCD
    CheckLCD -- 是 --> CHKRAM[检查内存]
    
    CHKRAM --> CheckRAM{日志含PASS?}
    CheckRAM -- 否 --> CHKRAM
    CheckRAM -- 是 --> CHKSD[检查SD读卡器]
    
    CHKSD --> CheckSD{CHKSD==PASS?}
    CheckSD -- 否 --> CHKSD
    CheckSD -- 是 --> CHKTP[检查触摸板]
    
    CHKTP --> CheckTP{CHKTP==PASS?}
    CheckTP -- 否 --> CHKTP
    CheckTP -- 是 --> CHKWLAN[检查无线网卡]
    
    CHKWLAN --> CheckWLAN{CHKWLAN==PASS?}
    CheckWLAN -- 否 --> CHKWLAN
    CheckWLAN -- 是 --> FANTest[检查风扇]
    
    FANTest --> CheckFAN{FANTest==PASS?}
    CheckFAN -- 否 --> FANTest
    CheckFAN -- 是 --> Final[设置RUNINCFG=PASS]
    
    style Start fill:#90EE90,stroke:#333
    style Final fill:#FFB6C1,stroke:#333
    style CheckDMI fill:#FFFF99,stroke:#666
    style CheckBatt fill:#FFFF99,stroke:#666
    style CheckBIOS fill:#FFFF99,stroke:#666

    subgraph 硬件检测流程
        SETDMI --> CHKBATT --> PreBIOS --> CHKBT --> CHKCAM --> CHKCPU --> CHKVGA --> CHKHDD
        CHKHDD --> CHKLAN --> CHKLCD --> CHKRAM --> CHKSD --> CHKTP --> CHKWLAN --> FANTest
    end