---
title: 6 Linux文件的打包和压缩.md
---

[toc]

## 6.1 压缩打包介绍

### 6.1.1 Window下常见的压缩文件后缀有 
* .rar
* .zip
* .7z

### 6.1.2 Linux下常见的压缩文件后缀有
* .zip
* .gz
* .bz2
* .xz
* .tar.gz
* .tar.bz2
* .tar.xz

## 6.2 gzip压缩工具
> gzip压缩工具不能压缩目录 , 这是gzip工具的一个特点
### 6.2.1 压缩文件
> 命令 : gzip {file}   gzip直接跟上文件名

![](http://oqjg6c4c1.bkt.clouddn.com/201708242232_706.png)

### 6.2.2 解压文件
> 命令 : gzip -d {file}
> 命令 : gunzip {file}

![](http://oqjg6c4c1.bkt.clouddn.com/201708242235_862.png)

![](http://oqjg6c4c1.bkt.clouddn.com/201708242241_418.png)


	1. 压缩后,目录下只剩下1.txt.gz
	2. 解压后,目录下只剩下1.txt 
	3. 也就是说 , 压缩后源文件不见了, 解压后 , 压缩文件不见了
	4. 压缩前, 1.txt 的大小是3.9M , 但是压缩后再解压 , 文件变成3.4M , 说明该文件有空隙,被压缩掉了
	5. 看到wc命令在压缩前后输出的文件的行数都是94930 , 说明, 即使文件压缩和解压后大小虽然变了,但是文件的内容没有改变

### 6.2.3 指定压缩级别
> 命令 : gzip -#  {file}  #的范围1-9  默认是6

![](http://oqjg6c4c1.bkt.clouddn.com/201708242244_731.png)

	1. 没有指定压缩级别的时候,压缩后的文件大小为920k
	2. 指定压缩级别为1的时候 , 压缩后的文件大小为1.1M

### 6.2.4 查看压缩文件的`压缩信息`
> file {compressed file name}

![](http://oqjg6c4c1.bkt.clouddn.com/201708242247_769.png)

### 6.2.5 查看压缩文件的内容
> zcat {compressed file name}  实际上,相当于先解压了,然后在cat

`90k多行, 实在无法截图了!`

### 6.2.6 压缩(解压)文件 , 不删除源文件 , 并且更换解压(压缩)后的文件路径
> gzip -c {file name} > /tmp/1.txt.gz
> gunzip -c {compressed file name} > /tmp/1.txt
> gunzip -c -d  {compressed file name} > /tmp/1.txt.new

![](http://oqjg6c4c1.bkt.clouddn.com/201708242255_866.png)


## 6.3 bzip2压缩工具
> 基本用户和gzip保持一致
> yum install -y bzip2 安装

![](http://oqjg6c4c1.bkt.clouddn.com/201708242307_786.png)
### 6.3.1 bzip2和gzip的区别
	1. bzip2的压缩比gzip更狠一点 , 默认的压缩级别是9 , 同时也说明了, bzip2在压缩和解压的工程中, 比gzip更加消耗cpu资源
	2. gzip能使用zcat查看压缩后的文件内容 , bzip2也能使用bzcat来查看

> bzip2  {file}
> bunzip2 {file}
> bzip2 -d
> bzip2 -c

## 6.4 xz压缩工具
> 使用方法和gzip基本保持一直 , 压缩率更高,耗费cpu资源更多

	1. xz 1.txt / xz -z 1.txt
	2. xz -d 1.txt.xz  /  unxz 1.txt.xz
	3. xz -# 1.txt   //#范围1-9 ,默认6
	4. 不能压缩目录
	5. xzcat 1.txt.xz
	6. xz -c 1.txt > /tmp/1.txt.xz
	7. xz -c -d /tmp/1.txt.xz > 1.txt.new3


## 6.5 zip压缩工具
> zip压缩工具安装 : yum install -y zip 

![](http://oqjg6c4c1.bkt.clouddn.com/201708261716_580.png)

> unzip解压工具安装 : yum install -y unzip

![](http://oqjg6c4c1.bkt.clouddn.com/201708261717_195.png)

> zip工具与其他压缩工具最大的区别在于 : zip工具可以压缩和解压目录

### 6.5.1 压缩文件
> 命令 : 不同于其他工具命令后直接跟要压缩的文件名 , zip工具要先指定压缩后的文件的名字,然后再指定要压缩的文件的名字 : `zip 1.txt.zip 1.txt`

== 错误指令 ==
```
[root@allinlinux-02 ys]# zip 1.txt
	zip warning: missing end signature--probably not a zip file (did you
	zip warning: remember to use binary mode when you transferred it?)
	zip warning: (if you are trying to read a damaged archive try -F)

zip error: Zip file structure invalid (1.txt)

```

== 正确指令 ==
```
[root@allinlinux-02 ys]# zip 1.txt.zip 1.txt
  adding: 1.txt (deflated 73%)
[root@allinlinux-02 ys]# ls
1.txt  1.txt.new  1.txt.zip  2.txt.new  yum

```
![](http://oqjg6c4c1.bkt.clouddn.com/201708261731_248.png)

	1. 压缩后 , 源文件不会消失

### 6.5.2压缩目录
> 命令 : zip -r {filename}.zip {filename}

```
1.txt  1.txt.new  1.txt.zip  2.txt.new  yum
[root@allinlinux-02 ys]# 
[root@allinlinux-02 ys]# ls
1.txt  1.txt.new  1.txt.zip  2.txt.new  yum
[root@allinlinux-02 ys]# du -sh /ys/yum/
3.4M	/ys/yum/
[root@allinlinux-02 ys]# tree yum/
yum/
├── 1.txt
├── fssnap.d
├── pluginconf.d
│   ├── fastestmirror.conf
│   └── langpacks.conf
├── protected.d
│   └── systemd.conf
├── vars
│   └── infra
└── version-groups.conf

4 directories, 6 files
[root@allinlinux-02 ys]# zip -r yum.zip yum/
  adding: yum/ (stored 0%)
  adding: yum/vars/ (stored 0%)
  adding: yum/vars/infra (stored 0%)
  adding: yum/pluginconf.d/ (stored 0%)
  adding: yum/pluginconf.d/fastestmirror.conf (deflated 28%)
  adding: yum/pluginconf.d/langpacks.conf (deflated 37%)
  adding: yum/fssnap.d/ (stored 0%)
  adding: yum/protected.d/ (stored 0%)
  adding: yum/protected.d/systemd.conf (stored 0%)
  adding: yum/version-groups.conf (deflated 40%)
  adding: yum/1.txt (deflated 73%)
[root@allinlinux-02 ys]# du -sh yum.zip 
924K	yum.zip
```
![](http://oqjg6c4c1.bkt.clouddn.com/201708261736_685.png)

![](http://oqjg6c4c1.bkt.clouddn.com/201708261737_598.png)

### 6.5.3解压文件
> 命令 : unzip {compressed file}.zip

```
[root@allinlinux-02 ys]# ls
1.txt  1.txt.new  1.txt.zip  2.txt.new  yum  yum.zip
[root@allinlinux-02 ys]# unzip 1.txt.zip
Archive:  1.txt.zip
replace 1.txt? [y]es, [n]o, [A]ll, [N]one, [r]ename: y
  inflating: 1.txt                   

```

![](http://oqjg6c4c1.bkt.clouddn.com/201708261741_155.png)


### 6.5.4解压目录
1. 解压到当前目录
> 命令 : unzip {compressed file}.zip
```
[root@allinlinux-02 ys]# ls
1.txt  1.txt.new  1.txt.zip  2.txt.new  yum  yum.zip
[root@allinlinux-02 ys]# unzip yum.zip 
Archive:  yum.zip
replace yum/vars/infra? [y]es, [n]o, [A]ll, [N]one, [r]ename: A
 extracting: yum/vars/infra          
  inflating: yum/pluginconf.d/fastestmirror.conf  
  inflating: yum/pluginconf.d/langpacks.conf  
 extracting: yum/protected.d/systemd.conf  
  inflating: yum/version-groups.conf  
  inflating: yum/1.txt     
  
```	

![](http://oqjg6c4c1.bkt.clouddn.com/201708261752_537.png)

2. 指定解压路径 , 不同于其他解压工具的是, zip可以指定解压路径, 但是不能指定解压后的文件的名字

> 命令 : unzip {compressed file}.zip -d /tmp/

```
[root@allinlinux-02 ys]# ls
1.txt  1.txt.new  1.txt.zip  2.txt.new  yum  yum.zip
[root@allinlinux-02 ys]# ls /tmp/
1.txt      systemd-private-4ceded13dc28450b9dae769e76908246-vmtoolsd.service-NqYRkH
1.txt.zip  systemd-private-7c91f137d411412f83eafe3d815e38a1-vmtoolsd.service-FFOKb5
2.txt.gz
[root@allinlinux-02 ys]# unzip yum.zip -d /tmp/
Archive:  yum.zip
   creating: /tmp/yum/
   creating: /tmp/yum/vars/
 extracting: /tmp/yum/vars/infra     
   creating: /tmp/yum/pluginconf.d/
  inflating: /tmp/yum/pluginconf.d/fastestmirror.conf  
  inflating: /tmp/yum/pluginconf.d/langpacks.conf  
   creating: /tmp/yum/fssnap.d/
   creating: /tmp/yum/protected.d/
 extracting: /tmp/yum/protected.d/systemd.conf  
  inflating: /tmp/yum/version-groups.conf  
  inflating: /tmp/yum/1.txt          
[root@allinlinux-02 ys]# ls /tmp/
1.txt      systemd-private-4ceded13dc28450b9dae769e76908246-vmtoolsd.service-NqYRkH
1.txt.zip  systemd-private-7c91f137d411412f83eafe3d815e38a1-vmtoolsd.service-FFOKb5
2.txt.gz   yum

```

![](http://oqjg6c4c1.bkt.clouddn.com/201708261755_530.png)

3.  , 即使在指定路径下还指定一个名字, 仍然会被当作是一个路径 , 若没有该路径, 则会先创建该路径

```
[root@allinlinux-02 ys]# unzip yum.zip -d /tmp/123.txt
Archive:  yum.zip
   creating: /tmp/123.txt/yum/
   creating: /tmp/123.txt/yum/vars/
 extracting: /tmp/123.txt/yum/vars/infra  
   creating: /tmp/123.txt/yum/pluginconf.d/
  inflating: /tmp/123.txt/yum/pluginconf.d/fastestmirror.conf  
  inflating: /tmp/123.txt/yum/pluginconf.d/langpacks.conf  
   creating: /tmp/123.txt/yum/fssnap.d/
   creating: /tmp/123.txt/yum/protected.d/
 extracting: /tmp/123.txt/yum/protected.d/systemd.conf  
  inflating: /tmp/123.txt/yum/version-groups.conf  
  inflating: /tmp/123.txt/yum/1.txt  
[root@allinlinux-02 ys]# ls /tmp/
123.txt  1.txt.zip  systemd-private-4ceded13dc28450b9dae769e76908246-vmtoolsd.service-NqYRkH  yum
1.txt    2.txt.gz   systemd-private-7c91f137d411412f83eafe3d815e38a1-vmtoolsd.service-FFOKb5
[root@allinlinux-02 ys]# ls /tmp/123.txt/
yum

```

![](http://oqjg6c4c1.bkt.clouddn.com/201708261759_286.png)


### 6.5.5 查看压缩文件内部结构
> 不同于其他压缩工具 , zip工具不能直接查看压缩后的文件内容, 只能查看压缩包里的内部结构
> >命令 : unzip -l {compressed file}.zip

```
[root@allinlinux-02 ys]# ls
1.txt  1.txt.new  1.txt.zip  2.txt.new  yum  yum.zip
[root@allinlinux-02 ys]# unzip -l 1.txt.zip
Archive:  1.txt.zip
  Length      Date    Time    Name
---------  ---------- -----   ----
  3529416  08-24-2017 22:29   1.txt
---------                     -------
  3529416                     1 file
[root@allinlinux-02 ys]# 

```

![](http://oqjg6c4c1.bkt.clouddn.com/201708261802_193.png)

```
[root@allinlinux-02 ys]# unzip -l yum.zip 
Archive:  yum.zip
  Length      Date    Time    Name
---------  ---------- -----   ----
        0  08-26-2017 17:22   yum/
        0  08-26-2017 17:20   yum/vars/
        6  08-26-2017 17:20   yum/vars/infra
        0  08-26-2017 17:20   yum/pluginconf.d/
      279  08-26-2017 17:20   yum/pluginconf.d/fastestmirror.conf
      372  08-26-2017 17:20   yum/pluginconf.d/langpacks.conf
        0  08-26-2017 17:20   yum/fssnap.d/
        0  08-26-2017 17:20   yum/protected.d/
        8  08-26-2017 17:20   yum/protected.d/systemd.conf
      444  08-26-2017 17:20   yum/version-groups.conf
  3529416  08-26-2017 17:22   yum/1.txt
---------                     -------
  3530525                     11 files
[root@allinlinux-02 ys]# 

```

![](http://oqjg6c4c1.bkt.clouddn.com/201708261803_764.png)


## 6.6 tar打包工具
> tar打包工具并不是压缩工具, 打包后的文件 , 其大小并没有很明显的变化

### 6.6.1 打包文件(打包单个文件貌似没什么意义)
> tar -cvf {packaged file}.tar {file}
	1. -c, --create               创建一个新归档
	2. -f, --file=ARCHIVE         使用归档文件或 ARCHIVE 设备
	-v, --verbose              详细地列出处理的文件
	
```
[root@allinlinux-02 ys]# ls
1.txt  1.txt.new  1.txt.zip  2.txt.new  yum  yum.zip
[root@allinlinux-02 ys]# du -sh 1.txt
3.4M	1.txt
[root@allinlinux-02 ys]# tar -cvf 1.txt.tar 1.txt
1.txt
[root@allinlinux-02 ys]# du -sh 1.txt.tar 
3.4M	1.txt.tar
[root@allinlinux-02 ys]# 

```

![](http://oqjg6c4c1.bkt.clouddn.com/201708261824_314.png)

### 6.6.2 打包多个文件

```
[root@allinlinux-02 ys]# tar -cvf 1.tar 1.txt 2.txt.new 1.txt.new 
1.txt
2.txt.new
1.txt.new

```
![](http://oqjg6c4c1.bkt.clouddn.com/201708261829_925.png)

### 6.6.3 打包目录

```
[root@allinlinux-02 ys]# tar -cvf yum.tar yum
yum/
yum/vars/
yum/vars/infra
yum/pluginconf.d/
yum/pluginconf.d/fastestmirror.conf
yum/pluginconf.d/langpacks.conf
yum/fssnap.d/
yum/protected.d/
yum/protected.d/systemd.conf
yum/version-groups.conf
yum/1.txt
[root@allinlinux-02 ys]# 

```

![](http://oqjg6c4c1.bkt.clouddn.com/201708261831_684.png)

### 6.6.4 打包目录和文件

```
[root@allinlinux-02 ys]# ls
1.tar  1.txt  1.txt.new  1.txt.tar  1.txt.zip  2.txt.new  yum  yum.tar  yum.zip
[root@allinlinux-02 ys]# tar -cvf test.tar yum 1.txt 2.txt.new 
yum/
yum/vars/
yum/vars/infra
yum/pluginconf.d/
yum/pluginconf.d/fastestmirror.conf
yum/pluginconf.d/langpacks.conf
yum/fssnap.d/
yum/protected.d/
yum/protected.d/systemd.conf
yum/version-groups.conf
yum/1.txt
1.txt
2.txt.new
[root@allinlinux-02 ys]# ls
1.tar  1.txt  1.txt.new  1.txt.tar  1.txt.zip  2.txt.new  test.tar  yum  yum.tar  yum.zip
[root@allinlinux-02 ys]# 

```

![](http://oqjg6c4c1.bkt.clouddn.com/201708261834_547.png)


### 6.6.5 打包目录,并排除指定的子目录或者文件(文件类型)

1. 排除子目录

```
[root@allinlinux-02 ys]# tree /ys/yum/
/ys/yum/
├── 1.txt
├── fssnap.d
├── pluginconf.d
│   ├── fastestmirror.conf
│   └── langpacks.conf
├── protected.d
│   └── systemd.conf
├── vars
│   └── infra
└── version-groups.conf

4 directories, 6 files

```

![](http://oqjg6c4c1.bkt.clouddn.com/201708261839_286.png)


```
[root@allinlinux-02 ys]# tar -cvf 111.tar yum --exclude vars
yum/
yum/pluginconf.d/
yum/pluginconf.d/fastestmirror.conf
yum/pluginconf.d/langpacks.conf
yum/fssnap.d/
yum/protected.d/
yum/protected.d/systemd.conf
yum/version-groups.conf
yum/1.txt
[root@allinlinux-02 ys]# 

```

![](http://oqjg6c4c1.bkt.clouddn.com/201708261840_589.png)


2. 排除某个文件 

![](http://oqjg6c4c1.bkt.clouddn.com/201708261845_933.png)

```
[root@allinlinux-02 ys]# tar -cvf 1.tar yum --exclude 2.txt
yum/
yum/vars/
yum/vars/infra
yum/pluginconf.d/
yum/pluginconf.d/fastestmirror.conf
yum/pluginconf.d/langpacks.conf
yum/fssnap.d/
yum/protected.d/
yum/protected.d/systemd.conf
yum/version-groups.conf
yum/1.txt
yum/3.txt
[root@allinlinux-02 ys]# 

```

![](http://oqjg6c4c1.bkt.clouddn.com/201708261846_408.png)

3. 排除某种类型的文件,如 .txt文件

![](http://oqjg6c4c1.bkt.clouddn.com/201708261847_324.png)

```
[root@allinlinux-02 ys]# tar -cvf 12.tar yum --exclude "*.txt"
yum/
yum/vars/
yum/vars/infra
yum/pluginconf.d/
yum/pluginconf.d/fastestmirror.conf
yum/pluginconf.d/langpacks.conf
yum/fssnap.d/
yum/protected.d/
yum/protected.d/systemd.conf
yum/version-groups.conf

```

![](http://oqjg6c4c1.bkt.clouddn.com/201708261848_939.png)

### 6.6.6 解包
> 命令 : tar -tf {packaged file}.tar

> tar 没有-d选项 , 不能指定解包的位置

```
[root@allinlinux-02 ys]# ls
111.tar  12.tar  1.tar  1.txt  1.txt.new  1.txt.tar  1.txt.zip  2.txt.new  test.tar  yum  yum.tar  yum.zip
[root@allinlinux-02 ys]# tar -tf 1.tar 
yum/
yum/vars/
yum/vars/infra
yum/pluginconf.d/
yum/pluginconf.d/fastestmirror.conf
yum/pluginconf.d/langpacks.conf
yum/fssnap.d/
yum/protected.d/
yum/protected.d/systemd.conf
yum/version-groups.conf
yum/1.txt
yum/3.txt

```

![](http://oqjg6c4c1.bkt.clouddn.com/201708261850_408.png)

## 6.7 打包并压缩

### 6.7.1 打包并且以gzip的格式压缩
> 命令 : tar -zcvf xxx.tar.gz xxx

```
[root@allinlinux-02 ys]# du -sh yum
3.4M	yum
[root@allinlinux-02 ys]# tar -zcvf yum.tar.gz yum
yum/
yum/vars/
yum/vars/infra
yum/pluginconf.d/
yum/pluginconf.d/fastestmirror.conf
yum/pluginconf.d/langpacks.conf
yum/fssnap.d/
yum/protected.d/
yum/protected.d/systemd.conf
yum/version-groups.conf
yum/1.txt
yum/2.txt
yum/3.txt
[root@allinlinux-02 ys]# du -sh yum.tar.gz 
920K	yum.tar.gz
[root@allinlinux-02 ys]# 

```

![](http://oqjg6c4c1.bkt.clouddn.com/201708282257_421.png)

### 6.7.2 解包并解压gzip格式的包
> 命令 : tar -zxvf xxx.tar.gz

```
[root@allinlinux-02 ys]# tar -zxvf yum.tar.gz 
yum/
yum/vars/
yum/vars/infra
yum/pluginconf.d/
yum/pluginconf.d/fastestmirror.conf
yum/pluginconf.d/langpacks.conf
yum/fssnap.d/
yum/protected.d/
yum/protected.d/systemd.conf
yum/version-groups.conf
yum/1.txt
yum/2.txt
yum/3.txt
[root@allinlinux-02 ys]# 

```

![](http://oqjg6c4c1.bkt.clouddn.com/201708282300_171.png)

### 6.7.3 打包并且以bzip2格式压缩
> 命令 : tar -jcvf  xxx.tar.bz2 xxx

```
[root@allinlinux-02 ys]# tar -jcvf yum.tar.bz2 yum
yum/
yum/vars/
yum/vars/infra
yum/pluginconf.d/
yum/pluginconf.d/fastestmirror.conf
yum/pluginconf.d/langpacks.conf
yum/fssnap.d/
yum/protected.d/
yum/protected.d/systemd.conf
yum/version-groups.conf
yum/1.txt
yum/2.txt
yum/3.txt
[root@allinlinux-02 ys]# du -sh yum.tar.bz2 
252K	yum.tar.bz2
[root@allinlinux-02 ys]# 

```

![](http://oqjg6c4c1.bkt.clouddn.com/201708282302_820.png)


### 6.7.4 解包并解压bzip2格式的包
> 命令 : tar -jxvf xxx.tar.bz2

```
[root@allinlinux-02 ys]# tar -jxvf yum.tar.bz2 
yum/
yum/vars/
yum/vars/infra
yum/pluginconf.d/
yum/pluginconf.d/fastestmirror.conf
yum/pluginconf.d/langpacks.conf
yum/fssnap.d/
yum/protected.d/
yum/protected.d/systemd.conf
yum/version-groups.conf
yum/1.txt
yum/2.txt
yum/3.txt
[root@allinlinux-02 ys]# 

```

![](http://oqjg6c4c1.bkt.clouddn.com/201708282305_725.png)

### 6.7.5 打包并且以xz格式压缩
> 命令 : tar -Jcvf xxx.tar.xz xxx

```
[root@allinlinux-02 ys]# tar -Jcvf yum.tar.xz yum
yum/
yum/vars/
yum/vars/infra
yum/pluginconf.d/
yum/pluginconf.d/fastestmirror.conf
yum/pluginconf.d/langpacks.conf
yum/fssnap.d/
yum/protected.d/
yum/protected.d/systemd.conf
yum/version-groups.conf
yum/1.txt
yum/2.txt
yum/3.txt
[root@allinlinux-02 ys]# du -sh yum.tar.xz 
40K	yum.tar.xz
[root@allinlinux-02 ys]# 

```

![](http://oqjg6c4c1.bkt.clouddn.com/201708282307_459.png)

### 6.7.6 解包并解压xz格式的包
> 命令 : tar -Jxvf xxx.tar.xz

```
[root@allinlinux-02 ys]# tar -Jxvf yum.tar.xz 
yum/
yum/vars/
yum/vars/infra
yum/pluginconf.d/
yum/pluginconf.d/fastestmirror.conf
yum/pluginconf.d/langpacks.conf
yum/fssnap.d/
yum/protected.d/
yum/protected.d/systemd.conf
yum/version-groups.conf
yum/1.txt
yum/2.txt
yum/3.txt
[root@allinlinux-02 ys]# 

```

![](http://oqjg6c4c1.bkt.clouddn.com/201708282308_812.png)

### 6.7.7 三种打包压缩方式,查看压缩后的文件结构
1. tar -tf xxx.tar.gz
```
[root@allinlinux-02 ys]# tar -tf yum.tar.gz
yum/
yum/vars/
yum/vars/infra
yum/pluginconf.d/
yum/pluginconf.d/fastestmirror.conf
yum/pluginconf.d/langpacks.conf
yum/fssnap.d/
yum/protected.d/
yum/protected.d/systemd.conf
yum/version-groups.conf
yum/1.txt
yum/2.txt
yum/3.txt
[root@allinlinux-02 ys]# 

```

![](http://oqjg6c4c1.bkt.clouddn.com/201708282311_993.png)

2. tar -tf xxx.tar.bz2

```
[root@allinlinux-02 ys]# tar -tf yum.tar.bz2
yum/
yum/vars/
yum/vars/infra
yum/pluginconf.d/
yum/pluginconf.d/fastestmirror.conf
yum/pluginconf.d/langpacks.conf
yum/fssnap.d/
yum/protected.d/
yum/protected.d/systemd.conf
yum/version-groups.conf
yum/1.txt
yum/2.txt
yum/3.txt
[root@allinlinux-02 ys]# 

```

![](http://oqjg6c4c1.bkt.clouddn.com/201708282311_329.png)


3. tar -tf xxx.tar.xz

```
[root@allinlinux-02 ys]# tar -tf yum.tar.xz
yum/
yum/vars/
yum/vars/infra
yum/pluginconf.d/
yum/pluginconf.d/fastestmirror.conf
yum/pluginconf.d/langpacks.conf
yum/fssnap.d/
yum/protected.d/
yum/protected.d/systemd.conf
yum/version-groups.conf
yum/1.txt
yum/2.txt
yum/3.txt
[root@allinlinux-02 ys]# 

```

![](http://oqjg6c4c1.bkt.clouddn.com/201708282312_619.png)







