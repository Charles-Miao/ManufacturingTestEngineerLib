- LIDdoNothing，盒盖之后不做任何事，不休眠，不关机，不睡眠

```batch
rem 查看嵌入式控制器0x612 寄存器的值
RDECRegX64.exe 0x612 | find "SET ECRegVal=00" && goto PASS
rem 设定嵌入式控制器0x612 寄存器的值
CoolBoot.exe 0x612 0x00 0x612 0x00
```

```mermaid
graph TD
    A[开始] --> B{检查 CPU 类型}
    B -->|Intel| C[执行 Intel 相关操作]
    B -->|AMD| D[执行 AMD 相关操作]
    B -->|未识别| E[未识别 CPU 类型]
    C --> F{检查寄存器值}
    D --> G{检查寄存器值}
    F -->|SET ECRegVal=00| H[设置成功，跳转到 PASS]
    G -->|SET ECRegVal=00| H
    F -->|其他| I[设置寄存器值并等待]
    G -->|其他| I
    I --> J[等待 2 秒]
    J --> K[重新开始]
    K --> A
    E --> L[暂停并显示信息]
    L --> M[跳转到 AMD 相关操作]
    H --> N[设置 LID 状态正常通过]
    N --> O[等待 2 秒]
    O --> P[结束]
