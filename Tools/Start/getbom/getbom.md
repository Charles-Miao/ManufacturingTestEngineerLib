- 如果StartTime为空，调用getbomST.bat
- getbomst.bat，相对getbom.bat多加了一个ST参数，以便获取%StartTime%

```mermaid
graph TD
    A[开始] --> B[断开WLAN连接]
    B --> C[添加WLAN配置文件]
    C --> D[尝试Ping服务器IP]
    D -->|Ping成功| E[读取MBSN]
    D -->|Ping失败| F[Ping服务器IP失败]
    F --> G[暂停]
    G --> A
    E --> H[上架操作]
    H -->|获取BOM成功| I[结束]
    H -->|获取BOM失败| J[获取BOM失败]
    J --> G
