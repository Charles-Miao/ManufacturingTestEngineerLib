```mermaid
graph TD
    A[开始] --> B[调用 BOM.bat]
    B --> C{IMAGEVER 是否为空?}
    C -->|是| E[结束]
    C -->|否| D[检查版本]
    D --> F{在 desktoplabel.ini 中找到 IMAGEVER 的第一部分?}
    F -->|是| G[PASS]
    F -->|否| H{在 desktoplabel.ini 中找到 IMAGEVER 的第二部分?}
    H -->|是| G
    H -->|否| I{在 desktoplabel.ini 中找到 IMAGEVER 的第三部分?}
    I -->|是| G
    I -->|否| J[FAIL]
    G --> K[延时 1 秒并重置颜色]
    K --> E
    J --> L[显示错误信息并暂停]
    L --> M[重新开始]
    M --> A
