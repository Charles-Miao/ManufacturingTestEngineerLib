- 大部分都和RUNINCFG相似，多两个项目，一个是CHKBIOS.BAT，一个是CHKMAC.BAT
```mermaid
graph TD
    Start[Start] --> Header[显示配置检查标题]
    Header --> CHKDMI[检查SSN/DMI]
    
    CHKDMI -->|CHKDMI!=PASS| CHKDMI
    CHKDMI --> CHKBATT[检查电池]
    
    CHKBATT -->|CHKBATT!=PASS| CHKBATT
    CHKBATT --> CHKBIOS[检查BIOS版本]
    
    CHKBIOS -->|CHKBIOS!=PASS| CHKBIOS
    CHKBIOS --> CHKBT[检查蓝牙]
    
    CHKBT -->|CHKBT!=PASS| CHKBT
    CHKBT --> CHKCAM[检查摄像头]
    
    CHKCAM -->|CHKCAM!=PASS| CHKCAM
    CHKCAM --> CHKCPU[检查CPU]
    
    CHKCPU -->|CHKCPU!=PASS| CHKCPU
    CHKCPU --> CHKVGA[检查显卡]
    
    CHKVGA -->|CHKVGA!=PASS| CHKVGA
    CHKVGA --> CHKHDD[检查硬盘]
    
    CHKHDD -->|日志无PASS| CHKHDD
    CHKHDD --> CHKLCD[检查LCD]
    
    CHKLCD -->|CHKLCD!=PASS| CHKLCD
    CHKLCD --> CHKRAM[检查内存]
    
    CHKRAM -->|日志无PASS| CHKRAM
    CHKRAM --> CHKSD[检查SD读卡器]
    
    CHKSD -->|CHKSD!=PASS| CHKSD
    CHKSD --> CHKTP[检查触摸板]
    
    CHKTP -->|CHKTP!=PASS| CHKTP
    CHKTP --> CHKWLAN[检查无线网卡]
    
    CHKWLAN -->|CHKWLAN!=PASS| CHKWLAN
    CHKWLAN --> CHKMAC[检查MAC地址]
    
    CHKMAC -->|CHKMAC!=PASS| CHKMAC
    CHKMAC --> FANTest[风扇测试]
    
    FANTest -->|复制测试工具| FANTestPrep
    FANTestPrep[复制FANTest工具] --> FANTestCheck
    FANTestCheck -->|FANTest!=PASS| FANTestCheck
    FANTestCheck --> SetPass[设置CFGCHK=PASS]
    
    classDef startend fill:#d9f2d9,stroke:#009933;
    classDef process fill:#e6f3ff,stroke:#0066cc;
    classDef condition fill:#ffe6cc,stroke:#ff9900;
    
    class Start,SetPass startend;
    class Header,CHKDMI,CHKBATT,CHKBIOS,CHKBT,CHKCAM,CHKCPU,CHKVGA,CHKHDD,CHKLCD,CHKRAM,CHKSD,CHKTP,CHKWLAN,CHKMAC,FANTestPrep process;
    class CHKDMI,CHKBATT,CHKBIOS,CHKBT,CHKCAM,CHKCPU,CHKVGA,CHKHDD,CHKLCD,CHKRAM,CHKSD,CHKTP,CHKWLAN,CHKMAC,FANTestCheck condition;