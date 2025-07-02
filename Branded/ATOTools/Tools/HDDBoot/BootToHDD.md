```mermaid
flowchart TD
    Start[Start] --> GetArg["获取设备号参数<br>devNumber = WScript.Arguments(0)"]
    GetArg --> InitArray["初始化数组<br>a = Array(&H11,0,&H01,0,&H01,0,0,0,0)<br>a(8) = devNumber"]
    InitArray --> ConnectWMI1["连接WMI服务<br>Set objWMIService = GetObject('winmgmts:\\\\.\\root\\WMI')"]
    
    ConnectWMI1 --> Query1["执行WMI查询<br>SELECT * FROM X86BIOSWMIPAG1"]
    Query1 --> Loop1["遍历查询结果"]
    
    Loop1 --> Echo1["输出分隔符和信息<br>Wscript.Echo '---'"]
    Echo1 --> Loop2["循环数组索引 (0-8)"]
    
    Loop2 --> SetPAG1["设置PAG1值<br>objItem.PAG1(i) = a(i)"]
    SetPAG1 --> Save["保存修改<br>objItem.Put_"]
    
    Save --> CheckIndex{"是否最后一个索引？"}
    CheckIndex --> |否| Loop2
    CheckIndex --> |是| NextItem1{"是否有下一个实例？"}
    
    NextItem1 --> |是| Loop1
    NextItem1 --> |否| ConnectWMI2["再次连接WMI服务"]
    
    ConnectWMI2 --> Query2["执行新查询<br>SELECT * FROM X86BIOSWMIExecuteWmiFunction"]
    Query2 --> Loop3["遍历查询结果"]
    
    Loop3 --> Execute["执行WMI函数<br>objItem.ExeMyWmiFn 1"]
    Execute --> Echo2["输出执行信息<br>Wscript.Echo 'ExeMyWmiFn'"]
    
    Echo2 --> NextItem2{"是否有下一个实例？"}
    NextItem2 --> |是| Loop3
    NextItem2 --> |否| End[End]