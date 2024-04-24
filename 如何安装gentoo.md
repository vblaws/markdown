[toc]



# 记录自己的Gentoo安装过程,做一个记录,有想装的也可以参考一下



## 网络

这个我是有线网,所以已经有了

## 磁盘分区

```shell
fdisk /devsda
下面都是fdisk里面的命令
g	# 转换成GPT分区表

# 以下是我的分区
/boot 500M	/dev/sda1
t # 修改分区类型
4	

/boot/efi	1G	/dev/sda2
t # 修改分区类型
输入uefi

/	剩余所有空间 /dev/sda1
不变,就是linux filesystem
```

```
将分区格式化
mkfs.ext4	/dev/sda1
mkfs.vfat -F 32 /dev/sda2
mkfs.ext4 /dev/sda3
```

```shell
挂载对应的分区
mount /dev/sda3	/mnt/gentoo	# 挂载根分区
mkdir -p /mnt/gentoo/boot
mount /dev/sda1 /mnt/gentoo/boot	# 挂载boot分区
mkdir -p /mnt/gentoo/boot/efi
mount /dev/sda2 /mnt/gentoo/boot/efi	# 挂载efi分区
```



## 安装stage3

我有这个包,位置在`D:\otherfile\isofile\`下面

解压到`/mnt/gentoo`目录下面


## 配置编译选项

```
root # nano /mnt/gentoo/etc/portage/make.conf
```

-  在这个文件里面配置Gentoo软件源

``` 
GENTOO_MIRRORS="https://mirrors.ustc.edu.cn/gentoo/"
```

-  配置编译设置

```
MAKEOPTS="-j4"
```



## Chrooting

### 复制DNS信息

```
root #cp --dereference /etc/resolv.conf /mnt/gentoo/etc/
```

### 进入chroot环境

-  由于我使用的是gentoo的安装媒介,所以可以直接执行`arch-chroot /mnt/gentoo`
-  source /etc/profile
-  export PS1="(chroot) ${PS1}" # 这个目的是让你意识到你正在chroot环境下

以下是原文

>  If using Gentoo's install media, this step can be replaced with simply: **arch-chroot /mnt/gentoo**.

>  如果安装Gentoo时在这一步之后的任何地方中断，那么“应该”可以从这一步“继续”安装。不必再重新给磁盘分区！只需要[挂载 root 分区](https://wiki.gentoo.org/wiki/Handbook:AMD64/Installation/Disks/zh-cn#Mounting_the_root_partition) 并运行上述步骤，然后通过[复制 DNS 信息](https://wiki.gentoo.org/wiki/Handbook:AMD64/Installation/Base/zh-cn#Copy_DNS_info)重新进入工作环境。 这也对修复引导程序问题很有用。更多的信息可以在 [chroot](https://wiki.gentoo.org/wiki/Chroot/zh-cn) 文章中找到。



### 配置 Portage

#### Gentoo ebuild 软件仓库

>  选择镜像的第二个重要步骤是通过/etc/portage/repos.conf/gentoo.conf文件来配置 Gentoo的 ebuild 软件仓库。这个文件包含了更新 Portage 数据库（包含 Portage 需要下载和安装软件包所需要的信息的一个 ebuild 和相关文件的集合）所需要的同步信息。

创建文件

```
root #mkdir --parents /mnt/gentoo/etc/portage/repos.conf
```

然后进入这个目录

`nano gentoo.conf`

将以下文件写入

```
[DEFAULT]
main-repo = gentoo
 
[gentoo]
location = /var/db/repos/gentoo
sync-type = rsync
sync-uri = rsync://rsync.mirrors.ustc.edu.cn/gentoo-portage
auto-sync = yes
sync-rsync-verify-jobs = 1
sync-rsync-verify-metamanifest = yes
sync-rsync-verify-max-age = 24
sync-openpgp-key-path = /usr/share/openpgp-keys/gentoo-release.asc
sync-openpgp-key-refresh-retry-count = 40
sync-openpgp-key-refresh-retry-overall-timeout = 1200
sync-openpgp-key-refresh-retry-delay-exp-base = 2
sync-openpgp-key-refresh-retry-delay-max = 60
sync-openpgp-key-refresh-retry-delay-mult = 4
sync-webrsync-verify-signature = yes
sync-git-verify-commit-signature = yes
```

### 从网站安装 Gentoo ebuild 数据库快照

这将从Gentoo的一个镜像中获取最新的快照（每天发布）并将其安装到系统上：

```
root #emerge-webrsync
```



### 选择正确的配置文件

运行 eselect 使用 profile模块，能看到当前系统正在使用什么配置文件：

```
root #eselect profile list
```



在看完框架的可用配置文件amd64之后，用户可以键入以下命令为系统选择一个不同的配置文件：

```
root #eselect profile set 2
```



### 配置 ACCEPT_LICENSE 变量

在`nano /mnt/gentoo/etc/portage/make.conf`中添加

```
ACCEPT_LICENSE="*"
```



### 更新@world集合

当系统应用了任何升级，或从 任何profile 构建了stage3 后，应用了变化的 use 标记时，下一步是必要的。

```
root#emerge --ask --verbose --update --deep --newuse @world
```

查看需要清除的东西

```
root#emerge --ask --pretend --depclean
```

把需要清除的东西清除

```
root#emerge --ask --depclean
```

### 时区

systemd

当使用 systemd 时，采用一种稍有不同的方法。生成一个符号链接：

```
root# ln -sf ../usr/share/zoneinfo/Europe/Brussels /etc/localtime
```

之后当 systemd 运行时，时区和相关设置可以使用 **timedatectl** 命令配置。

### 配置区域设置

系统支持的 locale 必须在 /etc/locale.gen 文件中定义。

```
root#nano /etc/locale.gen

在里面写入
en_US.UTF-8 UTF-8
zh_CN.UTF-8 UTF-8
```

下一步是运行 `locale-gen` 命令。此命令会生成 /etc/locale.gen 文件中所有指定的地区。

通过 **eselect locale list** 可显示可用的目标：

可以使用 **eselect locale set <NUMBER>** 选择正确的区域设置：

现在重新加载环境`root #env-update && source /etc/profile && export PS1="(chroot) ${PS1}"`

### 固件

#### Linux Firmware

在开始配置内核部分之前，最好了解下，一些硬件设备需要先在系统上安装附加的固件，有时是非自由及开放源代码软件（FOSS）才能正常运行。我们经常网络接口上会使用附加的固件，特别是无线网络接口，常用于台式电脑和笔记本电脑。此外，来自 AMD，Nvidia 和 Intel 等供应商的现代视频芯片，要想使用完整的功能，通常也需要外部固件文件。现代硬件设备的大多数固件都可以在 [sys-kernel/linux-firmware](https://packages.gentoo.org/packages/sys-kernel/linux-firmware) 软件包中找到。

为了在必要时提供固件，首次重启系统之前推荐安装 [sys-kernel/linux-firmware](https://packages.gentoo.org/packages/sys-kernel/linux-firmware) 软件包：

```
root #emerge --ask sys-kernel/linux-firmware
```

当为基于 amd64 的系统安装和编译内核时，Gentoo 推荐使用 [sys-kernel/gentoo-sources](https://packages.gentoo.org/packages/sys-kernel/gentoo-sources) 软件包。

选择一个合适的内核并使用 **emerge** 来安装它。

```
root #emerge --ask sys-kernel/gentoo-sources
```

首先，列出所有已安装的内核

```
root#eselect kernel list
```

然后，选择所有已安装的内核

```
root#eselect kernel set <Number>
```



说明完以及准备好之后，安装 [sys-kernel/genkernel](https://packages.gentoo.org/packages/sys-kernel/genkernel) 软件包：

```
root #emerge --ask sys-kernel/genkernel
```

运行**以下命令**来编译内核源码。值得注意的是，使用 **genkernel** 编译的内核适用于不同计算机体系结构的各种硬件，这可能使编译过程需要一阵子来完成。

```
root #genkernel --mountboot --install all
```

安装好以后,准备自动挂载开始编辑`/etc/fstab`文件

>  格式类似与这个样子
>
>  /dev/sda1   /efi        vfat    umask=0077     0 2
>  /dev/sda2   none         swap    sw                   0 0
>  /dev/sda3   /            xfs    defaults,noatime              0 1

加下来是配置网络和grub引导,这个晚上去学校写