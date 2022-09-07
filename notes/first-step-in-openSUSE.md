# openSUSE-RISCV的配置与安装

## 环境配置

我在Ubuntu 22.04上直接利用apt安装好了qemu和支持riscv64的一些相关组件。随后跟随 https://en.opensuse.org/openSUSE:RISC-V 上的**QEMU system emulation**一节进行安装。

从 https://download.opensuse.org/ports/riscv/tumbleweed/images/ 上下载镜像。我所采用的镜像是20220718 build1.7 XFCE。注意，wiki上的教程内，命令行中的文件名应根据实际下载的镜像文件名称作相应修改。

安装的过程中输入`virt-copy-out`一行命令时，提示

```
Command 'virt-copy-out' not found, but can be installed with:
sudo apt install guestfish
```

跟随提示安装guestfish后，执行命令

```
virt-copy-out -a openSUSE-Tumbleweed-RISC-V-XFCE-efi.riscv64-2022.07.18-Build1.7.qcow2 /boot .
```

输出

```
libguestfs: error: /usr/bin/supermin exited with error status 1.
To see full error messages you may need to enable debugging.
Do:
  export LIBGUESTFS_DEBUG=1 LIBGUESTFS_TRACE=1
and run the command again.  For further information, read:
  http://libguestfs.org/guestfs-faq.1.html#debugging-libguestfs
You can also run 'libguestfs-test-tool' and post the *complete* output
into a bug report or message to the libguestfs mailing list.
```

跟随提示操作之后，正常执行前文所述的命令。

教程上随后的一步操作是运行qemu，我将这几行复制下来写进run.sh中，并在开头添加一行`export qcow2image=openSUSE-Tumbleweed-RISC-V-XFCE-efi.riscv64-2022.07.18-Build1.7.qcow2`。执行`bash run.sh`。

执行失败，提示
```
boot/u-boot.bin: No such file or directory
qemu-system-riscv64: could not load kernel 'boot/u-boot.bin'
```

回头查看发现之前执行的命令输出中存在error：
```bash
...
supermin: kernel: modpath /lib/modules/5.15.0-41-generic
cp: cannot open '/boot/vmlinuz-5.15.0-41-generic' for reading: Permission denied
supermin: cp -p '/boot/vmlinuz-5.15.0-41-generic' '/var/tmp/.guestfs-1000/appliance.d.r5oo7k1s/kernel': command failed, see earlier errors
libguestfs: error: /usr/bin/supermin exited with error status 1, see debug messages above
libguestfs: trace: launch = -1 (error)
libguestfs: trace: close
libguestfs: closing guestfs handle 0x55f694678ae0 (state 0)
libguestfs: command: run: rm
libguestfs: command: run: \ -rf /tmp/libguestfs3ESLY7

```

尝试使用超级用户权限重新运行之前的命令，再`bash run.sh`

正常进入系统。

##运行

通过脚本启动后，通过`ssh root@localhost -p 22222`并输入密码进入系统。

```
aldehyde@ubuntu:~/openSUSE-riscv64$ ssh root@localhost -p 22222
(root@localhost) Password: 
Last login: Thu Jul 28 07:41:08 UTC 2022 from 10.0.2.2 on ssh
Have a lot of fun...
localhost:~ # 
```

todo: 如何运行用户界面？

upd1:
参考资料：[在 QEMU上的 openEuler RISC-V 中启动并测试 Xfce](https://github.com/isrc-cas/tarsier-oerv/tree/main/QA/Tests/Xfce)

由于一些不可抗力原因，笔者又重新配置了一遍环境。。。而qemu的环境和上述链接中的准备阶段"1.编译支持视频输出的 qemu"内容一致。

简单记录一下自己踩的坑：

* > ERROR: unknown option -enable-sdl
  *  `sudo apt install libsdl1.2-dev`
* > ERROR: Cannot find Ninja
  *  `sudo apt install ninja-build`
* >ERROR: User requested feature opengl
       configure was not able to find it.
       Please install epoxy with EGL
  * 前往https://github.com/anholt/libepoxy 下载源码并编译。可能需要安装额外的依赖：`sudo apt install meson libegl-mesa-dev`
* >ERROR: Dependency "virglrenderer" not found, tried pkgconfig
  * https://gitlab.freedesktop.org/virgl/virglrenderer  下载源码并编译。
  * >fatal error: gbm.h: 没有那个文件或目录
    * `sudo apt install libgbm-dev`,或者前往 https://github.com/robclark/libgbm 下载编译。
* > ERROR: Dependency "sdl2" not found, tried pkgconfig and config-tool
  * `sudo apt install libsdl2-dev`
* >ERROR: Dependency "gtk+-3.0" not found, tried pkgconfig
  * `sudo apt-get install libgtk-3-dev`

至此即可编译。笔者的环境是Ubuntu22.04.1。当然笔者的环境也可以不自己编译qemu，直接在apt上拉取需要的qemu版本`sudo apt install qemu-system-misc`.（只是折腾了一下）
也可以参考这篇教程：https://github.com/YunxiangLuo/testing/blob/main/Firefox/Firefox_installation_guide.md
