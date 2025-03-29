```batch
:EXITQR
@echo 123 >C:\EXITQR.log
```

- 此段代码是用于退出ShowQR的工具，有此文件则QR会自动退出
- 如果需要ShowQR，则需要提前删除C:\EXITQR.log
- 初始测试时，或者测试camera等功能时，会生成此文件，防止QR工具干扰测试