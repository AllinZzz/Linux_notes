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







