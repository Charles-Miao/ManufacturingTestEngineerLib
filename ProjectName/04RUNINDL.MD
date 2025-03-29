```mermaid
graph TD
    Start[Start] --> DisableFirewall[禁用防火墙]
    DisableFirewall --> SetVar[设置 ImageName 和 RI7Z 变量]
    SetVar --> CopyIMGitLoop[复制 IMGiT64 文件到 x:\wpetools]
    CopyIMGitLoop --> CheckRestoreCmd{存在 Restore.cmd?}
    CheckRestoreCmd -- 否 --> CopyIMGitLoop
    CheckRestoreCmd -- 是 --> RestoreImage[执行 Restore.cmd 恢复镜像]
    
    RestoreImage --> CheckRestoreResult{IMGiTFLG == PASS?}
    CheckRestoreResult -- 否 --> RES_FAIL[显示恢复失败警告]
    RES_FAIL --> PauseRestore[暂停并重启流程]
    PauseRestore --> Start
    
    CheckRestoreResult -- 是 --> Copy7zLoop[遍历 C-H 盘复制 7z 文件]
    Copy7zLoop --> Check7zCopied{存在 7z 文件?}
    Check7zCopied -- 否 --> Copy7zFail[显示复制失败]
    Copy7zFail --> Retry7z[重试复制]
    Retry7z --> Copy7zLoop
    
    Check7zCopied -- 是 --> CopyBOMLoop[遍历 C-H 盘复制 BOM.bat]
    CopyBOMLoop --> CheckBOMCopied{存在 BOM.bat?}
    CheckBOMCopied -- 否 --> CopyBOMFail[显示 BOM 失败]
    CopyBOMFail --> RetryBOM[重试复制]
    RetryBOM --> CopyBOMLoop
    
    CheckBOMCopied -- 是 --> LoadBIOS[set BIOS load default]
    LoadBIOS --> BootNormal[Set Boot Order Normal]
    BootNormal --> FinalSteps[记录日志/创建目录]
    FinalSteps --> End[End]
    
    style Start fill:#90EE90,stroke:#333
    style End fill:#FFB6C1,stroke:#333
    style RES_FAIL fill:#FFCCCB,stroke:#333
    style Copy7zFail fill:#FFCCCB,stroke:#333
    style CopyBOMFail fill:#FFCCCB,stroke:#333