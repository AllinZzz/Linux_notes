---
title: 7 Linux软件包安装和卸载
---
[toc]

## 7.1 安装软件包的三种方法
### 7.1.1 rmp工具
> rpm : redhat package manger . 假如安装A包需要依赖B包,安装B包又需要依赖C包, 那么要先安装C包 , 再安装B包 , 最后才能安装我们想要的A包

### 7.1.2 yum工具
> yum : Yellowdog Updater Modified . 实际上是python语言写的 , 最大特点是自动安装软件包所依赖的其他包

### 7.1.3 源码包
> 获得软件包的源代码 , 通过编译器编译成可执行的文件.

## 7.2 rpm包介绍
> 找到一个rpm包. 在安装CentOS7的时候 , 光驱里面有一个Package目录 , 下面就有很多rpm包

> rpm包格式 : 文件名 , 软件版本号 , 发布版本号 , 平台

```
[root@allinlinux-02 /]# mount /dev/cdrom /mnt
mount: /dev/sr0 写保护，将以只读方式挂载
[root@allinlinux-02 /]# cd /mnt/
[root@allinlinux-02 mnt]# ls
CentOS_BuildTag  EULA  images    LiveOS    repodata              RPM-GPG-KEY-CentOS-Testing-7
EFI              GPL   isolinux  Packages  RPM-GPG-KEY-CentOS-7  TRANS.TBL
[root@allinlinux-02 mnt]# 

```

![](http://oqjg6c4c1.bkt.clouddn.com/201708302218_486.png)

> ls /mnt/Packages 会看到很多以.rpm结尾的文件

![](http://oqjg6c4c1.bkt.clouddn.com/201708302220_851.png)

> tcp_wrappers-libs-7.6-77.el7.x86_64.rpm
> 1. tcp_wrappers-libs 文件名
> 2. 7.6 版本号
> 3. 77.el7 发布的版本号(也就是发布到什么版本的系统)
> 4. x86_64 cpu平台(架构)

> tex-fonts-hebrew-0.1-21.el7.noarch.rpm
> 1. tex-fonts-hebrew 文件名
> 2. 0.1 版本号
> 3. 21.el7 发布的版本号
> 4. noarch 与cpu平台(架构)无关,可以安装到任意cpu架构的机器上

## 7.3 rpm工具用法
### 7.3.1 安装
> 命令 : rpm -ivh rpm包文件 . 包文件即rpm包的全称
> 1. -i install
> 2. -v 可视化
> 3. -h 更人性化展示,(进度条)

```
[root@allinlinux-02 Packages]# rpm -ivh zsh-5.0.2-25.el7.x86_64.rpm 
准备中...                          ################################# [100%]
正在升级/安装...
   1:zsh-5.0.2-25.el7                 ################################# [100%]
[root@allinlinux-02 Packages]# 

```

![](http://oqjg6c4c1.bkt.clouddn.com/201708302245_662.png)

![](http://oqjg6c4c1.bkt.clouddn.com/201708302246_583.png)


### 7.3.2 升级
> 命令 : rpm -Uvh rpm包文件  一般情况下,都是有一个新的版本号发布
> 1. -U updata


### 7.3.3 卸载
> 命令 : rpm -e 包名  . 这里仅仅需要包名 , 不需要包文件

```
[root@allinlinux-02 Packages]#  
[root@allinlinux-02 Packages]#  rpm -e zsh-5.0.2-25.el7.x86_64.rpm 
错误：未安装软件包 zsh-5.0.2-25.el7.x86_64.rpm 
[root@allinlinux-02 Packages]# rpm -e zsh
[root@allinlinux-02 Packages]# 

```
![](http://oqjg6c4c1.bkt.clouddn.com/201708302250_579.png)


### 7.3.4 查询当前系统已安装的包
> 命令 : rpm -qa 
> 1. -q : query
> 2. -a  : all

```
几大屏幕这么多 , 无法复制了代码了, 截个图感受一下吧
```
![](http://oqjg6c4c1.bkt.clouddn.com/201708302253_59.png)


### 7.3.5 查询指定的包是否安装
> rpm -q 包名  . 这里也是只需要包名 , 不是包文件(全称)

```
[root@allinlinux-02 Packages]# rpm -q zsh
未安装软件包 zsh 
[root@allinlinux-02 Packages]# rpm -ivh zsh-5.0.2-25.el7.x86_64.rpm 
准备中...                          ################################# [100%]
正在升级/安装...
   1:zsh-5.0.2-25.el7                 ################################# [100%]
[root@allinlinux-02 Packages]# rpm -q zsh
zsh-5.0.2-25.el7.x86_64
[root@allinlinux-02 Packages]# 

```

![](http://oqjg6c4c1.bkt.clouddn.com/201708302255_847.png)
### 7.3.6 查询指定的包信息
> 命令 : rpm -qi 包名 
> 1. -q : query
> 2. -i : information

```
[root@allinlinux-02 Packages]# rpm -qi vim-enhanced
Name        : vim-enhanced
Epoch       : 2
Version     : 7.4.160
Release     : 1.el7_3.1
Architecture: x86_64
Install Date: 2017年06月02日 星期五 21时46分56秒
Group       : Applications/Editors
Size        : 2292098
License     : Vim
Signature   : RSA/SHA256, 2016年12月22日 星期四 01时14分11秒, Key ID 24c6a8a7f4a80eb5
Source RPM  : vim-7.4.160-1.el7_3.1.src.rpm
Build Date  : 2016年12月22日 星期四 01时00分52秒
Build Host  : c1bm.rdu2.centos.org
Relocations : (not relocatable)
Packager    : CentOS BuildSystem <http://bugs.centos.org>
Vendor      : CentOS
URL         : http://www.vim.org/
Summary     : A version of the VIM editor which includes recent enhancements
Description :
VIM (VIsual editor iMproved) is an updated and improved version of the
vi editor.  Vi was the first real screen-based editor for UNIX, and is
still very popular.  VIM improves on vi by adding new features:
multiple windows, multi-level undo, block highlighting and more.  The
vim-enhanced package contains a version of VIM with extra, recently
introduced features like Python and Perl interpreters.

Install the vim-enhanced package if you'd like to use a version of the
VIM editor which includes recently added enhancements like
interpreters for the Python and Perl scripting languages.  You'll also
need to install the vim-common package.
[root@allinlinux-02 Packages]# 

```

![](http://oqjg6c4c1.bkt.clouddn.com/201708302257_910.png)

![](http://oqjg6c4c1.bkt.clouddn.com/201708302257_323.png)


### 7.3.7 列出指定的包都安装了哪些文件
> 命令 : rpm -ql 包名 
> > 1. -q query
> > 2. -l  list

```
[root@allinlinux-02 Packages]# rpm -ql vim-enhanced
/etc/profile.d/vim.csh
/etc/profile.d/vim.sh
/usr/bin/rvim
/usr/bin/vim
/usr/bin/vimdiff
/usr/bin/vimtutor
[root@allinlinux-02 Packages]# 
```

![](http://oqjg6c4c1.bkt.clouddn.com/201708302259_745.png)


### 7.3.8 查看一个文件是由哪个包安装的
> 命令 : rpm -qf 文件的绝对路径 
>> 1. -q query 
>> 2. -f  file

```
[root@allinlinux-02 Packages]# rpm -qf /etc/profile.d/vim.csh
vim-enhanced-7.4.160-1.el7_3.1.x86_64
[root@allinlinux-02 Packages]# 

```

![](http://oqjg6c4c1.bkt.clouddn.com/201708302302_163.png)



