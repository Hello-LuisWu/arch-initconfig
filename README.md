# arch-initconfig

### 换国内镜像源

编辑 `/etc/pacman.d/mirrorlist` 文件, 在顶部添加以下内容

```sh
#清华源
Server = https://mirrors.tuna.tsinghua.edu.cn/archlinux/$repo/os/$arch
#阿里源
Server = http://mirrors.aliyun.com/archlinux/$repo/os/$arch
#中科大源
Server = https://mirrors.ustc.edu.cn/archlinux/$repo/os/$arch
```
`sudo pacman -Syyu` 更新源 缓存


**或者运行以下命令更换:**

```sh
# 查看能用的源镜像排名，并选择速度较快的一个或多个源
sudo pacman-mirrors -i -c China -m rank

查看选择的源
sudo vim /etc/pacman.d/mirrorlist

# 更新源
sudo pacman -Syyu
```

### 添加非官方 的源

用 `vim /etc/pacman.conf` 命令打开 `pacman.conf` 文件也可以写在`/etc/pacman.d/mirrorlist`

> 注：pacman.conf 是 pacman 的总主控配置；mirrorlist 只是单独存放「所有镜像服务器地址」的引用文件，被 pacman.conf 导入使用。
> 控制 pacman 全部全局行为、仓库定义、安全规则、下载行为，是包管理器的主配置入口

#### archlinuxcn  

Arch Linux 中文社区仓库 是由 Arch Linux 中文社区驱动的非官方用户仓库。包含中文用户常用软件、工具、字体/美化包等。

完整的包信息列表（包名称/架构/维护者/状态）请 [点击这里 ](https://github.com/archlinuxcn/repo) 查看 

 官方仓库地址：https://repo.archlinuxcn.org

```sh
# 添加源 

[archlinuxcn]
SigLevel = Optional TrustedOnly
#清华源
Server = https://mirrors.tuna.tsinghua.edu.cn/archlinuxcn/$arch

#中科大源
Server = https://mirrors.ustc.edu.cn/archlinuxcn/$arch

# 官方源
Server   = http://repo.archlinuxcn.org/$arch

# 163源
Server = http://mirrors.163.com/archlinux-cn/$arch

# 阿里
Server = https://mirrors.aliyun.com/archlinuxcn/$arch
```

安装一些软件时提示gpg签名错误损坏等问题，是没有导入key的原因，我们要导入GPG KEY

```sh
# 导入 archlinuxcn key
sudo pacman -Sy archlinuxcn-keyring
```

#### antergosarchlinux软件仓库的源

```sh
# antergos 已经废弃
# [antergos]
# SigLevel = TrustAll
# Server = https://mirrors.tuna.tsinghua.edu.cn/antergos/$repo/$arch
```

#### 4edu

Arch4edu 一个面向全球高校用户的社区源, 初衷是实验室的服务器都有装同一批软件的需求，后来干脆就做成软件源了. 支持 Arch Linux 和 Arch Linux ARM， 主要包含高校用户常用的科研、教学及开发软件。

现在arch4edu主要涵盖以下方向的包:  
- 机器学习工具：tensorflow, caffe, torch等等
- IDE及编辑器：android-studio, pycharm, vs code, sublime等等
- Android开发：android-studio, android-sdk, android-ndk等等
- 语音信号处理：kaldi, cmusphinx, opensmile等等(我实验室的方向)
- 图像处理：opencv-git
- 通用：anaconda, zotero, atlas-lapack, openblas等等

共104个包。理论上说至少大学生常用的，不限于上述方向的包都可以加，欢迎大家发Package Request

现在的更新频率是日更。由于结构是仿照archlinuxcn搭建的，所以git版本的包会及时更新。

项目地址：https://github.com/arch4edu/arch4edu

镜像地址：https://mirrors.tuna.tsinghua.edu.cn/arch4edu

```sh
# 导入4edu GPG key
# pacman-key --recv-keys 7931B6D628C8D3BA
# pacman-key --finger 7931B6D628C8D3BA
# pacman-key --lsign-key 7931B6D628C8D3BA
```

```sh
#  添加 4edu 源
[arch4edu]
SigLevel = TrustAll
Server = https://mirrors.tuna.tsinghua.edu.cn/arch4edu/$arch
```

执行 `pacman -Sy` 即可 

#### Archlinux repo-ck
repo-ck 是 Arch 的非官方仓库，内有包含 ck 补丁、BFS 调度器等，通用或为特定 CPU 架构优化过的内核，以及内核相关的软件包，是居家旅行，优化折腾的必备良药。更多内容，参考 [ArchWiki](https://wiki.archlinux.org/index.php/repo-ck)

在 `/etc/pacman.conf` 里添加

```sh
[repo-ck]
Server = https://mirrors.tuna.tsinghua.edu.cn/repo-ck/$arch
```

再增加 GPG 信任：

```sh
pacman-key -r 5EE46C4C && pacman-key --lsign-key 5EE46C4C
```

执行 `pacman -Sy` 即可 

#### BlackArch

BlackArch 是一款基于 ArchLinux 的为渗透测试及安全研究人员开发的发行版，相当于 Arch 版的 BackTrack/Kali。
仓库地址：(https://blackarch.org/blackarch/)

收录架构 i686, x86_64, ARM 相关（目前包含 armv6h/armv7h/aarch64）

在 `/etc/pacman.conf` 里添加

```sh
[blackarch]
Server = https://mirrors.ustc.edu.cn/blackarch/$repo/os/$arch

```

再增加 GPG 信任：

```sh
sudo pacman -S blackarch-keyring
```

执行 `pacman -Sy` 即可 

相关链接: [manjaro 源](https://www.cnblogs.com/vconlln/articles/17065413.html)

---
<br/>

### 安装 base-devel

通过 yay 可以获取到更多的软件仓库资源

arch Linux 核心‌开发工具包组。使用 AUR（Arch 用户软件仓库）的必备前提：AUR 是 Arch 社区维护的、由用户贡献的海量软件脚本库。这些脚本（PKGBUILD）需要你在自己的电脑上编译软件。而 base-devel 正是执行这个编译过程所默认已安装的基础环境。  
base-devel 是 Arch 生态中从源代码构建软件的核心工具集，特别是在使用 AUR 时不可或缺。
.
**包含核心工具:**

- 编译器：如 GCC（GNU编译器套件）。
- 构建工具：如 make、automake 等。
- 版本控制：如  git。
- 其他工具：如 patch、sudo 等。

```sh
sudo pacman -S base-devel
```

### 安装 yay

yay(Yet Another Yogurt) 是 pacman 的封装工具，会自动同时检索官方仓库（pacman 源） + AUR：
包存在官方源 → 内部调用 sudo pacman 安装
仅存在 AUR → 自动拉取 PKGBUILD、编译安装
所有 pacman 参数 -S/-R/-Q/-Syu 在 yay 里通用，不用分开敲两条命令。
安装 yay 之前，必须确保系统已安装 base-devel 和 git 这两个基础工具包。
yay 的安装过程本身就是通过 makepkg（包含在 base-devel 中）从 AUR 的 PKGBUILD 文件构建的。

```sh
sudo pacman -S  yay
```
