- 此脚本是start起来的，前端脚本会和其平行执行，前端脚本会有键盘输入
- 如果出现空格键，则mode设定为auto，line_name则读取A B C D E F G H I J K L M N V X Z
- 如果出现回车键，则mode设定为manual
- 最终结果会写入到ReadLine.cmd中，前端脚本会读取ReadLine.cmd中的内容

```mermaid
graph TD
    A[开始] --> B[删除ReadLine.cmd和KeyData目录内容]
    B --> C[以最小化窗口启动OUBOKB.exe]
    C --> D[进入循环]
    D --> E[等待1秒]
    E --> F{OUBOKB.exe是否在运行?}
    F -->|否| G[跳过处理]
    F -->|是| H{KeyData目录下是否存在SPACE.txt?}
    H -->|是| I[设置Mode为auto并跳转到line_name]
    H -->|否| J{KeyData目录下是否存在RETURN.txt?}
    J -->|是| K[设置Mode为manual并跳转到end]
    J -->|否| D
    I --> L[line_name处理]
    L --> M{KeyData目录下是否存在特定字母的.txt文件?}
    M -->|是| N[设置line_name并跳转到end]
    M -->|否| D
    K --> O[end处理]
    N --> O
    O --> P[生成ReadLine.cmd文件]
    P --> Q[结束脚本]
    G --> R[强制结束OUBOKB.exe和CameraScan.exe进程]
    R --> S[暂停并退出]
