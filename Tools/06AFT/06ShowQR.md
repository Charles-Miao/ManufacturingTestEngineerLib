```mermaid
flowchart TD
    A[开始] --> B[初始化环境]
    B --> C[删除旧文件]
    C --> D[调用BOM.BAT]
    D --> E[生成SN.log并启动ShowSN.exe]
    E --> F[循环开始]
    F --> G{检测EXITQR.log?}
    G -- 存在 --> H[结束流程]
    G -- 不存在 --> I[生成QR.log]
    I --> J{检测KBtest.txt中SET KB=START?}
    J -- 存在 --> K[启动desktoplabel.exe]
    K --> L[终止ShowBarCode.exe]
    L --> M[启动ShowBarCode.exe]
    J -- 不存在 --> N[比较QR.log和QRO.log]
    N -- 相同 --> F
    N -- 不同 --> M
    M --> O[复制QR.log为QRO.log]
    O --> F
    H --> P[终止ShowBarCode.exe和ShowSN.exe]
    P --> Q[退出]