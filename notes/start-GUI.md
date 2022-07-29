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

检查启动虚拟机的log，发现以下报错：

```bash
         Starting Rule-based Manage…for Device Events and Files...
[   89.453080][    C0] watchdog: BUG: soft lockup - CPU#0 stuck for 28s! [modprobe:893]
[  120.131554][    C0] watchdog: BUG: soft lockup - CPU#0 stuck for 56s! [modprobe:893]
[  148.121845][    C0] watchdog: BUG: soft lockup - CPU#0 stuck for 82s! [modprobe:893]
[ TIME ] Timed out waiting for device /dev/hvc0.
[DEPEND] Dependency failed for Serial Getty on hvc0.
[  OK  ] Finished Monitoring of LVM… dmeventd or progress polling.
[FAILED] Failed to start Rule-based…r for Device Events and Files.
See 'systemctl status systemd-udevd.service' for details.
[FAILED] Failed to start Flush Journal to Persistent Storage.
See 'systemctl status systemd-journal-flush.service' for details.
[ TIME ] Timed out waiting for device /dev/ttyS0.
[DEPEND] Dependency failed for Serial Getty on ttyS0.
[ TIME ] Timed out waiting for device d-4a77-887f-35ec7bbc76e3.
[DEPEND] Dependency failed for /dev…9-70bd-4a77-887f-35ec7bbc76e3.
[DEPEND] Dependency failed for Swaps.
[ TIME ] Timed out waiting for device /dev/disk/by-uuid/A9F8-46F0.
[DEPEND] Dependency failed for /boot/efi.
[DEPEND] Dependency failed for Local File Systems.
```

尚不清楚图形界面未初始化是否与之有关。