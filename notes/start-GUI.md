##尝试启动用户界面

在Robin的提示下，在启动脚本中加入

virtio-vga vnc/spice

这些服务

于是，在启动脚本末尾添加如下两行

```bash
   -device virtio-vga \
   -vnc :5
```

安装remote-viewer，启动虚拟机后在另一终端中输入`remote-viewer vnc://localhost:5905`，得到img1中的结果

输出

> Guest has not initialized display (yet).

