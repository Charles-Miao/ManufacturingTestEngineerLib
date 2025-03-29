```mermaid
graph TD
    A[Start] --> B[直接跳转PASS]
    B --> C[设置CHKLAN=PASS]
    C --> D[流程结束]

    style C fill:#cfc,stroke:#090

    subgraph 被跳过的实际检测流程
        S[Start标签] --> T[显示检测标题]
        T --> U[获取PCI设备列表]
        U --> V{双VID/PID检查}
        V --> |匹配任意组合| PASS
        V --> |不匹配| W{特定Realtek检查}
        W --> |10EC&8186存在| PASS
        W --> |不存在| X[记录错误]
        X --> Y[显示错误并循环]
        Y --> S
    end

    style X fill:#f9f,stroke:#333