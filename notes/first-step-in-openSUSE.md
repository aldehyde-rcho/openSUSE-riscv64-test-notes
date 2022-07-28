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
aldehyde@ubuntu:~/openSUSE-riscv64$ virt-copy-out -a openSUSE-Tumbleweed-RISC-V-XFCE-efi.riscv64-2022.07.18-Build1.7.qcow2 /boot .
libguestfs: trace: set_verbose true
libguestfs: trace: set_verbose = 0
libguestfs: create: flags = 0, handle = 0x55f694678ae0, program = virt-copy-out
libguestfs: trace: set_pgroup true
libguestfs: trace: set_pgroup = 0
libguestfs: trace: add_drive "openSUSE-Tumbleweed-RISC-V-XFCE-efi.riscv64-2022.07.18-Build1.7.qcow2" "readonly:true"
libguestfs: creating COW overlay to protect original drive content
libguestfs: trace: get_tmpdir
libguestfs: trace: get_tmpdir = "/tmp"
libguestfs: trace: disk_create "/tmp/libguestfs3ESLY7/overlay1.qcow2" "qcow2" -1 "backingfile:/home/aldehyde/openSUSE-riscv64/openSUSE-Tumbleweed-RISC-V-XFCE-efi.riscv64-2022.07.18-Build1.7.qcow2"
libguestfs: trace: disk_format "/home/aldehyde/openSUSE-riscv64/openSUSE-Tumbleweed-RISC-V-XFCE-efi.riscv64-2022.07.18-Build1.7.qcow2"
libguestfs: command: run: qemu-img --help | grep -sqE -- '\binfo\b.*-U\b'
libguestfs: command: run: qemu-img
libguestfs: command: run: \ info
libguestfs: command: run: \ -U
libguestfs: command: run: \ --output json
libguestfs: command: run: \ /home/aldehyde/openSUSE-riscv64/openSUSE-Tumbleweed-RISC-V-XFCE-efi.riscv64-2022.07.18-Build1.7.qcow2
libguestfs: parse_json: qemu-img info JSON output:\n{\n    "virtual-size": 5377097728,\n    "filename": "/home/aldehyde/openSUSE-riscv64/openSUSE-Tumbleweed-RISC-V-XFCE-efi.riscv64-2022.07.18-Build1.7.qcow2",\n    "cluster-size": 65536,\n    "format": "qcow2",\n    "actual-size": 1841963008,\n    "format-specific": {\n        "type": "qcow2",\n        "data": {\n            "compat": "1.1",\n            "compression-type": "zlib",\n            "lazy-refcounts": false,\n            "refcount-bits": 16,\n            "corrupt": false,\n            "extended-l2": false\n        }\n    },\n    "dirty-flag": false\n}\n\n
libguestfs: trace: disk_format = "qcow2"
libguestfs: command: run: qemu-img
libguestfs: command: run: \ create
libguestfs: command: run: \ -f qcow2
libguestfs: command: run: \ -o backing_file=/home/aldehyde/openSUSE-riscv64/openSUSE-Tumbleweed-RISC-V-XFCE-efi.riscv64-2022.07.18-Build1.7.qcow2,backing_fmt=qcow2
libguestfs: command: run: \ /tmp/libguestfs3ESLY7/overlay1.qcow2
Formatting '/tmp/libguestfs3ESLY7/overlay1.qcow2', fmt=qcow2 cluster_size=65536 extended_l2=off compression_type=zlib size=5377097728 backing_file=/home/aldehyde/openSUSE-riscv64/openSUSE-Tumbleweed-RISC-V-XFCE-efi.riscv64-2022.07.18-Build1.7.qcow2 backing_fmt=qcow2 lazy_refcounts=off refcount_bits=16
libguestfs: trace: disk_create = 0
libguestfs: trace: add_drive = 0
libguestfs: trace: is_config
libguestfs: trace: is_config = 1
libguestfs: trace: launch
libguestfs: trace: max_disks
libguestfs: trace: max_disks = 255
libguestfs: trace: version
libguestfs: trace: version = <struct guestfs_version = major: 1, minor: 46, release: 2, extra: , >
libguestfs: trace: get_backend
libguestfs: trace: get_backend = "direct"
libguestfs: launch: program=virt-copy-out
libguestfs: launch: version=1.46.2
libguestfs: launch: backend registered: unix
libguestfs: launch: backend registered: uml
libguestfs: launch: backend registered: libvirt
libguestfs: launch: backend registered: direct
libguestfs: launch: backend=direct
libguestfs: launch: tmpdir=/tmp/libguestfs3ESLY7
libguestfs: launch: umask=0002
libguestfs: launch: euid=1000
libguestfs: trace: get_cachedir
libguestfs: trace: get_cachedir = "/var/tmp"
libguestfs: begin building supermin appliance
libguestfs: run supermin
libguestfs: command: run: /usr/bin/supermin
libguestfs: command: run: \ --build
libguestfs: command: run: \ --verbose
libguestfs: command: run: \ --if-newer
libguestfs: command: run: \ --lock /var/tmp/.guestfs-1000/lock
libguestfs: command: run: \ --copy-kernel
libguestfs: command: run: \ -f ext2
libguestfs: command: run: \ --host-cpu x86_64
libguestfs: command: run: \ /usr/lib/x86_64-linux-gnu/guestfs/supermin.d
libguestfs: command: run: \ -o /var/tmp/.guestfs-1000/appliance.d
supermin: version: 5.2.1
supermin: package handler: debian/dpkg
supermin: acquiring lock on /var/tmp/.guestfs-1000/lock
supermin: build: /usr/lib/x86_64-linux-gnu/guestfs/supermin.d
supermin: reading the supermin appliance
supermin: build: visiting /usr/lib/x86_64-linux-gnu/guestfs/supermin.d/base.tar.gz type gzip base image (tar)
supermin: build: visiting /usr/lib/x86_64-linux-gnu/guestfs/supermin.d/daemon.tar.gz type gzip base image (tar)
supermin: build: visiting /usr/lib/x86_64-linux-gnu/guestfs/supermin.d/excludefiles type uncompressed excludefiles
supermin: build: visiting /usr/lib/x86_64-linux-gnu/guestfs/supermin.d/hostfiles type uncompressed hostfiles
supermin: build: visiting /usr/lib/x86_64-linux-gnu/guestfs/supermin.d/init.tar.gz type gzip base image (tar)
supermin: build: visiting /usr/lib/x86_64-linux-gnu/guestfs/supermin.d/packages type uncompressed packages
supermin: build: visiting /usr/lib/x86_64-linux-gnu/guestfs/supermin.d/packages-hfsplus type uncompressed packages
supermin: build: visiting /usr/lib/x86_64-linux-gnu/guestfs/supermin.d/packages-reiserfs type uncompressed packages
supermin: build: visiting /usr/lib/x86_64-linux-gnu/guestfs/supermin.d/packages-xfs type uncompressed packages
supermin: build: visiting /usr/lib/x86_64-linux-gnu/guestfs/supermin.d/udev-rules.tar.gz type gzip base image (tar)
supermin: mapping package names to installed packages
supermin: resolving full list of package dependencies
supermin: build: 216 packages, including dependencies
supermin: build: 8426 files
supermin: build: 4965 files, after matching excludefiles
supermin: build: 4968 files, after adding hostfiles
supermin: build: 4965 files, after removing unreadable files
supermin: build: 4971 files, after munging
supermin: kernel: looking for kernel using environment variables ...
supermin: kernel: looking for kernels in /lib/modules/*/vmlinuz ...
supermin: kernel: looking for kernels in /boot ...
supermin: kernel: kernel version of /boot/vmlinuz-5.15.0-41-generic = 5.15.0-41-generic (from filename)
supermin: kernel: picked modules path /lib/modules/5.15.0-41-generic
supermin: kernel: kernel version of /boot/vmlinuz-5.15.0-40-generic = 5.15.0-40-generic (from filename)
supermin: kernel: picked modules path /lib/modules/5.15.0-40-generic
supermin: kernel: picked vmlinuz /boot/vmlinuz-5.15.0-41-generic
supermin: kernel: kernel_version 5.15.0-41-generic
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
