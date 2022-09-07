##尝试启动用户界面

在Robin的提示下，在启动脚本中加入

virtio-vga vnc/spice

这些服务

于是，在启动脚本末尾添加如下两行

```bash
   -device virtio-vga \
   -display sdl -vga std \
   -vnc :5
```

安装remote-viewer，启动虚拟机后在另一终端中输入`remote-viewer vnc://localhost:5905`，得到img1中的结果

输出

> Guest has not initialized display (yet).

我所下载的openSUSE包默认安装了X11,通过ssh连接到虚拟机的root上之后运行`startx`，报错：

```log
(EE) 
Fatal server error:
(EE) no screens found(EE) 
(EE) 
Please consult the The X.Org Foundation support 
	 at http://wiki.x.org
 for help. 
(EE) Please also check the log file at "/var/log/Xorg.0.log" for additional information.
(EE) 
VGA Arbitration: Cannot restore default device.
(EE) Server terminated with error (1). Closing log file.
xinit: giving up
xinit: unable to connect to X server: Connection refused
xinit: server error
-------------------------------------------------------------------------------------------
xinit failed. /usr/bin/Xorg is not setuid, maybe that's the reason?
If so either use a display manager (strongly recommended) or adjust /etc/permissions.local and run "chkstat --system --set" afterward
```

检查`/var/log/Xorg.o.log`

```log
localhost:~ # cat /var/log/Xorg.0.log | grep EE
[   167.443] (EE) open /dev/dri/card0: No such file or directory
[   167.447] (EE) open /dev/fb0: No such file or directory
[   167.448] (EE) open /dev/dri/card0: No such file or directory
[   167.452] (EE) open /dev/fb0: No such file or directory
[   167.452] (EE) No devices detected.
[   167.454] (EE) 
[   167.458] (EE) no screens found(EE) 
[   167.462] (EE) 
[   167.462] (EE) Please also check the log file at "/var/log/Xorg.0.log" for additional information.
[   167.464] (EE) 
[   167.470] (EE) Server terminated with error (1). Closing log file.

```