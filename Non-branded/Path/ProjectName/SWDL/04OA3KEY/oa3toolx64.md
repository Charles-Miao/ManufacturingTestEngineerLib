```batch
:: 生成OA3激活文件
oa3toolx64.exe /assemble /configfile=oa3.cfg
# 作用：组装MSDM二进制文件(OA3.bin)和XML配置文件
# 参数说明：
#   /assemble    - 从BOM信息生成激活文件
#   /configfile  - 指定包含产品密钥和硬件信息的配置文件
# 输出文件：
#   OA3.bin      - 写入BIOS的激活数据
#   OA3.xml      - 包含激活信息的XML文件

:: 生成CBR报告
oa3toolx64.exe /report /Configfile=oa3.cfg /LogTrace=oa3log-Report.xml
# 参数说明：
# /report       - 生成硬件哈希和XML报告
# /Configfile   - 指定OA3.cfg配置文件
# /LogTrace     - 生成详细操作日志

:: 验证硬件哈希（脚本中已注释）
::oa3toolx64.exe /ValidateHwHash=.\CBR\Report.xml
# 作用：验证CBR报告中的硬件哈希是否符合微软认证要求
# 参数说明：
#   /ValidateHwHash - 检查以下关键字段：
#     - SystemManufacturer
#     - SystemProductName
#     - TPM版本
#     - 内存容量
#     - 磁盘容量
# 验证标准：
#   通过 → 硬件符合Windows Autopilot部署要求
#   失败 → 无法进行OEM激活，需检查硬件配置

:: 解码硬件哈希
oa3toolx64.exe /decodehwhash=.\CBR\Report.xml /LogTrace=validateCBR.xml
# 参数说明：
# /decodehwhash - 验证并解码硬件哈希
# validateCBR.xml - 输出验证结果日志
```

```batch
OEM 激活工具 3.0
(c) 版权所有 2023 Microsoft Corp.

版本：10.0.26100.1

    OA3Tool.exe {/Assemble | /Report | /Return}
                /ConfigFile=<配置文件名及路径>

    OA3Tool.exe /Report
                /ConfigFile=<配置文件名及路径>
                [/LogTrace=<日志报告文件>]

    OA3Tool.exe /DecodeHwHash={<报告文件> | <base64字符串>}
                [/LogTrace=<报告文件>]

    OA3Tool.exe /CheckEdition {/Online | /ImageDrive=<镜像驱动器>}

    OA3Tool.exe /Validate

    OA3Tool.exe /ValidateSMBIOS{=<SMBIOS原始数据值>}

    OA3Tool.exe /ValidateHwHash={<报告文件> | <base64字符串>}
                [/LogTrace=<报告文件>]

功能描述：
    该工具用于在工厂环境中组装、生成报告并回传用于OEM计算机激活的唯一标识符。该标识符基于产品密钥、硬件哈希、OEM ID以及微软和OEM的附加信息（包括语言、程序等）生成。

主要命令选项：
    /Assemble     - 从密钥提供程序获取产品密钥，组装MSDM二进制文件(OA3.bin)和XML文件(OA3.xml)
    /Report       - 创建OA3.xml并生成硬件哈希，向密钥提供程序回传激活信息
    /Return       - 将激活信息回传给密钥提供程序

诊断专用命令：
    /DecodeHwHash - 将报告文件中的Base64编码硬件哈希解码为XML格式
    /CheckEdition - 验证注入密钥的版本与预装Windows版本是否匹配
    /Validate     - 验证MSDM表结构完整性（存在性、必要字段、格式合规性）
    /ValidateSMBIOS - 验证SMBIOS结构中TotalPhysicalRAM和PrimaryDiskTypeCapacity字段的初始化情况
    /ValidateHwHash - 验证硬件哈希是否符合Autopilot功能的关键字段要求及许可证费用计算的重要字段标准

配置文件说明：
    /ConfigFile   - 指定配置文件路径，该文件包含：
                    - 密钥提供程序信息
                    - OA3.bin和OA3.xml的输出路径
                    - 其他激活相关配置参数
```