# 这篇文章旨在交大家如何配置archlinux,让他可以可以进行日常的使用

注意:这篇教程已经默认你安装好了archlinux,如果还不会安装的话，看我的另一篇文章

> 我arch版本是是[**archlinux2024.3.29 X86-64**](https://mirrors.aliyun.com/archlinux/iso/2024.03.29/archlinux-2024.03.29-x86_64.iso?spm=a2c6h.25603864.0.0.19cb51c4H2mObT)
> 桌面环境是**KDE 6.0.3**

## 先第一步就是把arch切换成中文

!注意:下面的编辑文件用的都是vim,可以使用`sudo pacman -S vim`来下载

1.首先`vim /eyc/locale.gen`打开这个文件
    - 找到zh_CN UTF-8 UTF-8去掉前面的`#`号,
    - 然后执行`sudo locale-gen`这个命令

> 这几条命令的作用是让`arch`支持中文

2.然后下载字体文件,只要选择一个就可以了,格式`sudo pacman -S 字体名`.

```txt
adobe-source-han-sans-cn-fonts
adobe-source-han-serif-cn-fonts
noto-fonts-cjk
wqy-microhei
wqy-microhei-lite
wqy-bitmapfont
wqy-zenhei
ttf-arphic-ukai
ttf-arphic-uming
```

3.最后按下windows键打开菜单栏,点击System setting,

## 第二步就是如何使用中文输入法

- 需要安装以下的软件

```shell
sudo pacman -S fcitx5-im
sudo pacman -S fcitx5-chinese-addons  fcitx5-rime
```

> fcitx5-chinese-addons 包含了大量中文输入方式：拼音、双拼、五笔拼音、自然码、仓颉、冰蟾全息、二笔等
fcitx5-rime 对经典的 Rime IME 输入法的包装，内置了繁体中文和简体中文的支持。

- 然后在环境变量配置文件`sudo vim /etc/environment`中添加如下内容

```shell
GTK_IM_MODULE=fcitx
QT_IM_MODULE=fcitx
XMODIFIERS=@im=fcitx
INPUT_METHOD=fcitx
SDL_IM_MODULE=fcitx
GLFW_IM_MODULE=ibus
```

- 都设置好了以后,**重启**.点击右下角类似于键盘的东西,然后右键,点击配置

- 在右下角找到添加输入法,找到拼音,添加就可以了

## 使用Arch 用户仓库（AUR）

什么是 AUR？

> AUR 表示 Arch 用户仓库(Arch User Repository)。它是针对基于 Arch 的 Linux 发行版用户的社区驱动的仓库。它包含名为 PKGBUILD 的包描述，它可让你使用 makepkg 从源代码编译软件包，然后通过 pacman（Arch Linux 中的软件包管理器）安装。

在Arch上安装AUR助手

> 假设你要使用 Yay AUR 助手。确保在 Linux 上安装了 git。然后克隆仓库，进入目录并构建软件包。

- 这里首先要将yay文件夹添加777权限

```shell
chmod 777 yay/
```

- 再将里面的PKGBUILD添加可执行权限

```shell
chmod a+x PKGBUILD
```

然后执行一下命令

```shell
sudo pacman -S git
sudo git clone https://aur.archlinux.org/yay.git
cd yay
makepkg -si
```

其余:安装AstroNvim和zsh
>如果想要使用AstroNvim的话,为了完整的显示,需要下载Nerd Font字体文件

当然你一开始有可能会发现时间怎么不对,所以需要设置时区和打开时间同步服务,命令如下

```shell
timedatectl set-timezone Asia/Shanghai
timedatectl set-ntp true
```

然后你就会发现时间变正常了

---

本文所有参考的文章
[美化arch](https://www.cnblogs.com/Junglezt/p/16927100.html)
