##尝试openSUSE.riscv上运行libreoffice

在这之前，一个尚未解决的问题是：如何运行GUI。

不管那么多了，先试着在系统中输入命令libreoffice：

```
localhost:~ # libreoffice
If 'libreoffice' is not a typo you can use command-not-found to lookup the package that contains it, like this:
    cnf libreoffice
localhost:~ # cnf libreoffice
libreoffice: searching ... 
Warning: incomplete repos found but could not refresh - try to refresh manually, e.g. with 'zypper refresh'.
 libreoffice: command not found                            
localhost:~ # 
```

进一步在 build.opensuse.org 上以libreoffice riscv为关键词搜索，发现一个链接：
https://build.opensuse.org/project/show/home:Andreas_Schwab:riscv:libreoffice

显示软件包构建失败。logfile部分截图见附件img2。