`CoolBoot.exe`是一个命令行工具，用于向嵌入式控制器（EC）发送特定的命令，以使笔记本电脑（NB）进入 S5 状态。S5 状态通常是指系统关闭（Shutdown）状态，这是 ACPI（高级配置和电源接口）定义的电源管理状态之一。


1.指令用途
`CoolBoot.exe`的主要用途是通过向 EC 的特定寄存器写入特定值，触发系统进入 S5 状态。这通常用于硬件调试、系统维护或自动化测试场景中，以确保系统能够正确关闭。


2.指令格式

```plaintext
CoolBoot.exe <arg1> <arg2> <arg3> <arg4>
```


• `<arg1>`：第一个寄存器的 RAM 偏移地址，以十六进制（HEX）格式表示。

• `<arg2>`：写入第一个寄存器的值，以十六进制（HEX）格式表示。

• `<arg3>`：第二个寄存器的 RAM 偏移地址，以十六进制（HEX）格式表示。

• `<arg4>`：写入第二个寄存器的值，以十六进制（HEX）格式表示。


3.示例用法

```plaintext
CoolBoot.exe 0x4E7 0x3C 0x4E8 0x01
```


• 这个命令会向寄存器地址`0x4E7`写入值`0x3C`，并向寄存器地址`0x4E8`写入值`0x01`，从而触发系统进入 S5 状态。


4.帮助命令

```plaintext
CoolBoot.exe /?
```


• 这个命令会显示`CoolBoot.exe`的帮助信息，提供如何使用该工具的详细说明。


5.示例输出

```plaintext
CoolBoot.exe  Send command to EC to make NB going into S5 State.
Usage:
  CoolBoot.exe <arg1> <arg2> <arg3> <arg4>
  argv1: the register ram offset of send the S5 command 1 to
  argv2: the value of command 1
  argv3: the register ram offset of send the S5 command 2 to
  argv4: the value of command 2
Sample:
  CoolBoot.exe 0x4E7 0x3C 0x4E8 0x01
```



6.使用场景

• 硬件调试：开发人员可以使用这个工具在硬件调试过程中触发系统关闭，以验证硬件在关闭状态下的行为。

• 系统维护：系统管理员可以使用这个工具在维护过程中安全地关闭系统。

• 自动化测试：可以将这个工具集成到自动化测试脚本中，以测试系统在关闭状态下的行为。


7.注意事项

• 权限问题：运行`CoolBoot.exe`时，可能需要管理员权限。确保以管理员身份运行命令提示符。

• 工具路径：确保`CoolBoot.exe`的路径已添加到系统环境变量中，或者在命令行中指定完整路径。

• 寄存器地址和值：确保提供的寄存器地址和值是有效的，并且符合硬件规格。

• 系统状态：在使用这个工具之前，确保系统处于可以安全关闭的状态，以避免数据丢失或硬件损坏。


8.示例批处理脚本
以下是一个简单的批处理脚本示例，展示如何使用`CoolBoot.exe`触发系统进入 S5 状态：


```batch
@echo off
:: 调用 CoolBoot.exe 触发系统进入 S5 状态
C:\TOOLS\LIDdoNothing\CoolBoot.exe 0x4E7 0x3C 0x4E8 0x01

:: 输出提示信息
echo System is now in S5 state.

:: 暂停并等待用户输入
pause
```



9.总结
`CoolBoot.exe`是一个实用的工具，用于向嵌入式控制器发送特定命令，以使系统进入 S5 状态。通过简单的命令行操作，可以快速触发系统关闭，便于硬件调试、系统维护或自动化测试。

希望这些信息对你有所帮助！如果还有其他疑问，请随时告诉我。