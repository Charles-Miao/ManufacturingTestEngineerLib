```mermaid
graph TD
    Start[Start] --> CallBOM[调用BOM.bat]
    CallBOM --> ExecResponse[执行response.cmd]
    ExecResponse --> SetParams[设置产品参数]
    
    SetParams --> GenInfoFile[生成产品信息文件]
    GenInfoFile --> GetUUID[获取系统UUID]
    GetUUID --> ExtractHash[提取HW_Hash]
    
    ExtractHash --> CreateDir[创建目标目录]
    CreateDir --> CopyTXT[复制.txt文件]
    CopyTXT -->|失败| RetryTXT[暂停并重试]
    RetryTXT --> CopyTXT
    CopyTXT --> CopyXML[复制.xml报告]
    
    CopyXML -->|失败| RetryXML[暂停并重试]
    RetryXML --> CopyXML
    CopyXML --> SetPass[设置UploadCBR=PASS]

    classDef process fill:#e6f3ff,stroke:#0066cc;
    classDef condition fill:#ffe6cc,stroke:#ff9900;
    classDef data fill:#d9f2d9,stroke:#009933;
    classDef error fill:#ffd9cc,stroke:#ff3300;
    
    class Start,SetPass process;
    class CallBOM,ExecResponse,SetParams,GenInfoFile,GetUUID,ExtractHash,CreateDir,CopyTXT,CopyXML process;
    class CopyTXT,CopyXML condition;
    class RetryTXT,RetryXML error;
    class GenInfoFile,ExtractHash data;