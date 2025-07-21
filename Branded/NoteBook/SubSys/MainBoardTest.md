```mermaid
graph TD
    classDef startend fill:#F5EBFF,stroke:#BE8FED,stroke-width:2px;
    classDef process fill:#E5F6FF,stroke:#73A6FF,stroke-width:2px;
    classDef decision fill:#FFF6CC,stroke:#FFBC52,stroke-width:2px;

    A([开始]):::startend --> B(记录测试开始时间):::process
    B --> C(记录测试项开始时间):::process
    C --> D(获取主板测试配置信息):::process
    D --> E(清空测试项结果列表):::process
    E --> F(初始化测试状态为通过):::process
    F --> G(创建 SMBIOS 实体对象):::process
    G --> H{检查 AC 电源状态}:::decision
    H -- 未连接 --> I(标记测试失败):::process
    I --> J(发送错误消息):::process
    J --> Z(TestEnd):::process
    H -- 连接 --> K{检查驱动器状态}:::decision
    K -- 失败 --> L(标记测试失败):::process
    L --> M(发送错误消息):::process
    M --> Z
    K -- 成功 --> N(发送驱动器检查通过消息):::process
    N --> O(根据客户配置切换数据):::process
    O --> P{启用 RTC 测试?}:::decision
    P -- 是 --> Q(显示 RTC 测试子项信息):::process
    Q --> R{执行 RTC 测试是否失败?}:::decision
    R -- 是 --> I
    R -- 否 --> S{初始化 SMBIOS 数据是否失败?}:::decision
    P -- 否 --> S
    S -- 是 --> I
    S -- 否 --> T{检查 BIOS 信息是否失败?}:::decision
    T -- 是 --> I
    T -- 否 --> U{检查 CPU 信息是否失败?}:::decision
    U -- 是 --> I
    U -- 否 --> V{检查内存信息是否失败?}:::decision
    V -- 是 --> I
    V -- 否 --> W{启用 PD FW 检查?}:::decision
    W -- 是 --> X{执行 PD FW 检查是否失败?}:::decision
    X -- 是 --> I
    X -- 否 --> Y{检查 GPU 信息是否失败?}:::decision
    W -- 否 --> Y
    Y -- 是 --> I
    Y -- 否 --> Z1{启用 DC 启动检查?}:::decision
    Z1 -- 是 --> Z2{执行 DC 启动检查是否失败?}:::decision
    Z2 -- 是 --> I
    Z2 -- 否 --> Z3{启用温度传感器检查?}:::decision
    Z1 -- 否 --> Z3
    Z3 -- 是 --> Z4(重试 3 次执行温度传感器检查):::process
    Z4 --> Z5{最终状态是否失败?}:::decision
    Z5 -- 是 --> I
    Z5 -- 否 --> Z6{启用 VR FW 检查?}:::decision
    Z3 -- 否 --> Z6
    Z6 -- 是 --> Z7(重试 3 次执行 VR FW 检查):::process
    Z7 --> Z8{最终状态是否失败?}:::decision
    Z8 -- 是 --> I
    Z8 -- 否 --> Z
    Z --> AA(显示测试最终结果信息):::process
    AA --> AB(添加主板测试结果到结果列表):::process
    AB --> AC(返回最终测试结果):::process
    AC --> AD([结束]):::startend