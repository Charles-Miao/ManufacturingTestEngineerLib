- 此CameraScan.exe工具用于扫描二维码（获取自动测试，线别等信息），并生成log
- 配合Line.bat使用，如果存在readline.cmd，则结束
- 如果log中存在manual，也结束
- 否则获取线别信息，若是auto，则结束

```mermaid
graph TD
    A[Start] --> B[chcp 65001]
    B --> C[start /min LINE.bat]
    C --> D[color f]
    D --> E[del /q ReadLine.cmd]
    E --> F[del /q Result.log]
    F --> G[set line_name=A]
    G --> H[set n=0]
    H --> I[loop]
    I --> J{Is n == 300?}
    J --Yes--> K[end]
    J --No--> L[Increment n]
    L --> M[del Result.log /q]
    M --> N[call CameraScan.exe]
    N --> O[set /p result=<Result.log]
    O --> P[timeout 1]
    P --> Q{ReadLine.cmd Exists?}
    Q --Yes--> R[call ReadLine.cmd]
    R --> S[Go to END]
    Q --No--> T{Result.log Contains 'manual'?}
    T --Yes--> S
    T --No--> U[Check Line Name]
    U --> V[Loop through A to Z]
    V --> W{Result Contains Line Letter?}
    W --Yes--> X[Set line_name=Line Letter]
    X --> Y[Go to mode]
    W --No--> Z[Continue Loop]
    Z --> I
    Y --> mode
    mode --> AA{Result.log Exists?}
    AA --No--> I
    AA --Yes--> AB{Result.log Contains 'auto'?}
    AB --Yes--> S
    AB --No--> I

    S --> AC[END]
    AC --> AD[taskkill /f /im OUBOKB.exe]
    AD --> AE[timeout 1]
    AE --> K
