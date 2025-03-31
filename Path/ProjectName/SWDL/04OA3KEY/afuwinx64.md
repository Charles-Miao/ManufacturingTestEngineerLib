
```batch
AFUWINx64 /aOA3.bin /JBC  
# 使用 OA3.bin 文件进行 OEM 激活
# /a 参数：指定 OEM 激活文件 (OA3.bin)
# /JBC 参数：跳过电源适配器和电池检查

afuwinx64.exe /oad /JBC
# 删除现有的 OEM 激活密钥
# /oad 参数：删除 OEM 激活密钥
# /JBC 参数：同上，跳过电源检查
```

```batch
+---------------------------------------------------------------------------+
|                 AMI 固件更新工具 v5.16.02.0111                            |
|      版权所有 (c) 1985-2023, American Megatrends International LLC.       |
|          保留所有权利。需遵守AMI许可协议。                                 |
+---------------------------------------------------------------------------+
| 用法: AFUWINx64.exe <ROM文件名> [选项1] [选项2]...                        |
|           或                                                              |
|        AFUWINx64.exe <输入或输出文件名> <命令>                           |
|           或                                                              |
|        AFUWINx64.exe <命令>                                               |
| ------------------------------------------------------------------------- |
| 命令:                                                                     |
|        /FV - 将安全固件卷复制到EFI                                        |
|         /O - 将当前ROM镜像保存到文件                                      |
|         /U - 显示ROM文件的ROM标识符                                       |
|         /S - 参考选项: /S                                                 |
|         /D - 验证给定ROM文件而不刷写BIOS                                  |
|        /A: - 参考选项: /A:                                               |
|       /OAD - 参考选项: /OAD                                              |
| /CLNEVNLOG - 参考选项: /CLNEVNLOG                                        |
| 选项:                                                                     |
|      /CMD: - 向BIOS发送特殊命令。 /CMD:{xxx}                              |
|   /OEMCMD: - 向BIOS发送特殊值。 /OEMCMD:xxx                              |
|       /DPC - 不检查Aptio 4和Aptio 5平台                                   |
|       /PW: - 输入文件密码                                                 |
|     /MEUL: - 编程ME完整固件块，支持生产.BIN和预生产.BIN文件               |
|         /Q - 静默执行                                                     |
|         /X - 不检查ROM ID                                                 |
|         /S - 显示当前系统的ROM标识符                                      |
|       /JBC - 不检查电源适配器和电池                                        |
|  /HOLEOUT: - 根据RomHole GUID保存特定ROM孔洞                              |
|              示例: NewRomHole1.BIN /HOLEOUT:GUID                          |
|        /SP - 保留BIOS设置                                                 |
|         /R - 编程时保留所有SMBIOS结构                                     |
|        /Rn - 编程时保留类型n的SMBIOS结构(n=0-255)                         |
|         /B - 编程引导块                                                   |
|         /P - 编程主BIOS                                                   |
|         /N - 编程NVRAM                                                    |
|         /K - 编程所有非关键块                                             |
|        /Kn - 编程第n个非关键块(n=0-15)                                    |
|      /RLC: - 设置ROM布局变更的默认选项                                   |
|              (E:整个BIOS区域, A:中止, F:强制, M:混合非BIOS区域命令)      |
|    /CLRCFG - 编程时不保留设置配置                                          |
|    /BCPALL - 刷写前保存所有问题值                                         |
|    /MLANG: - 支持多国语言映射                                             |
|     /HOLE: - 根据RomHole GUID更新特定ROM孔洞                               |
|              示例: NewRomHole1.BIN /HOLE:GUID                             |
|  /K:<GUID> - 按GUID编程非关键块                                           |
|         /L - 编程所有ROM孔洞                                              |
|        /Ln - 仅编程第n个ROM孔洞(n=0-15)                                   |
|      /ECUF - 检测到新版本时更新EC BIOS                                    |
|         /E - 编程嵌入式控制器块                                           |
|        /ME - 编程ME完整固件块                                             |
|       /FDR - 刷写FDR区域                                                  |
|      /DER2 - 刷写DER2区域                                                 |
|       /MER - 刷写MER区域                                                  |
|      /MEUF - 编程ME点火固件块                                             |
|        /A: - OEM激活文件                                                  |
|       /OAD - 删除OEM激活密钥                                              |
| /CLNEVNLOG - 清除事件日志                                                 |
|   /CAPSULE - 将安全刷写策略覆盖为胶囊更新                                 |
|  /RECOVERY - 将安全刷写策略覆盖为恢复更新                                 |
|        /EC - 编程嵌入式控制器块（闪存类型）                               |
|    /REBOOT - 编程完成后重启                                               |
|  /SHUTDOWN - 编程完成后关机                                              |
+---------------------------------------------------------------------------+

 处理完成。
 ```