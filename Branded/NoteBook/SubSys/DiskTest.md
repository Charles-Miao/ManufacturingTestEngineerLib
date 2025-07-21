```mermaid
graph TD
    classDef startend fill:#F5EBFF,stroke:#BE8FED,stroke-width:2px;
    classDef process fill:#E5F6FF,stroke:#73A6FF,stroke-width:2px;
    classDef decision fill:#FFF6CC,stroke:#FFBC52,stroke-width:2px;
    
    A([开始]):::startend --> B(清空测试项结果列表):::process
    B --> C(初始化测试状态为通过):::process
    C --> D(记录测试开始时间):::process
    D --> E(声明本地硬盘信息列表):::process
    E --> F{获取本地硬盘信息是否成功?}:::decision
    F -- 否 --> G(设置测试状态为失败):::process
    G --> H(跳转到测试结束):::process
    F -- 是 --> I(过滤移动硬盘，只保留非移动硬盘):::process
    I --> J{是否找到本地硬盘?}:::decision
    J -- 否 --> K(设置测试状态为失败):::process
    K --> L(发送错误消息):::process
    L --> H
    J -- 是 --> M(将过滤后的结果转换为列表):::process
    M --> N{是否启用硬盘数量检查?}:::decision
    N -- 是 --> O(发送硬盘数量检查信息):::process
    O --> P{硬盘数量是否匹配?}:::decision
    P -- 否 --> Q(发送错误消息):::process
    Q --> R(设置测试状态为失败):::process
    R --> H
    P -- 是 --> S(发送通过消息):::process
    S --> T{是否启用硬盘总容量检查?}:::decision
    N -- 否 --> T
    T -- 是 --> U{是否设置了容量上下限?}:::decision
    U -- 是 --> V(计算本地硬盘总容量):::process
    V --> W{总容量是否低于下限?}:::decision
    W -- 是 --> X(设置测试状态为失败):::process
    W -- 否 --> Y{总容量是否高于上限?}:::decision
    X --> Y
    Y -- 是 --> X
    Y -- 否 --> Z(添加硬盘总容量测试结果):::process
    Z --> AA(发送硬盘总容量检查信息):::process
    AA --> AB{总容量检查是否通过?}:::decision
    AB -- 否 --> AC(发送错误消息):::process
    AC --> H
    AB -- 是 --> AD(发送通过消息):::process
    AD --> AE{是否启用硬盘信息检查?}:::decision
    U -- 否 --> AF(设置测试状态为失败):::process
    AF --> AG(发送错误消息):::process
    AG --> H
    T -- 否 --> AE
    AE -- 是 --> AH(检查 MES 中的硬盘信息):::process
    AH --> AI(发送硬盘信息检查结果消息):::process
    AI --> AJ{硬盘信息检查是否通过?}:::decision
    AJ -- 否 --> H
    AJ -- 是 --> AK(注释部分为 SSD 固件更新逻辑，暂时未启用):::process
    AK --> AL(显示测试结果):::process
    AE -- 否 --> AL
    AL --> AM(添加最终测试结果):::process
    AM --> AN(返回测试结果对象):::process
    AN --> A1([结束]):::startend
    H --> AL