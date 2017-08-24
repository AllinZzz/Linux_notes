---
title: 5 Vim工具.md
---

# Vim

## Vim介绍
* vim是vi的升级版

* vim是带有颜色显示的

	> vim的颜色显示 , 与其文件的内容 , 类型(后缀名) , 所在位置有关.在 /etc/ 目录下的文件 , 一般都是配置文件 , 大部分都有高亮和颜色的显示 . 但是如果把/etc/passwd文件移动到/tmp/目录下, 那么使用vim去打开改文件 , 也会没有颜色显示 . 还有其他一些因素会影响vim的颜色显示 .

	vi:
	![](http://oqjg6c4c1.bkt.clouddn.com/201708222141_73.png)
	
	vim:
	![](http://oqjg6c4c1.bkt.clouddn.com/201708222142_572.png)

* yum install -y vim-enhanced


* 一般模式 , 编辑模式 , 命令模式
	*  一般模式 : 在未按下i,a等键编辑文件之前, 使用dd删除一行 , 复制某行到指定的位置 , 等都是在一般模式下操作
	*  编辑模式 : 在按下i,a等键编辑文件 , 可以往文件中输入任意字符
	*  命令模式 : 在使用(/+字符) , 在文件中搜索等的时候 , 就是命令模式


## 一般模式

### 移动光标

按键 | 作用
--- | ---
h或者向左方向键 | 光标向左移动一个字符
l(小写的字母L)或者向右方向键 | 光标向右移动一个字符
j或者向下方向键 | 光标向下移动一个字符
k或者向上方向键 | 光标向上移动一个字符
空格键 | 向左移动一个字符
n+空格键(先按一个数字,再按空格键) | 向左移动n个字符
n+h/j/k/l | 向左/下/上/右移动n个字符
ctrl+f或者PageUp键 | 屏幕向前移动一页
ctrl+b或者PageDown键 | 屏幕向后移动一页
数字0或者shift+6 | 移动到本行行首
Shift+4 | 移动到本行行尾
gg | 移动到首行
G | 移动到尾行
nG(n是任意数字) | 移动到第n行

### 复制粘贴

按键 | 作用
--- | ---
x,X | x表示向后删除(剪切)一个字符 ; X表示向前删除(剪切)一个字符
nx | 向后删除n个字符
dd | 删除/剪切光标所在的那一行
ndd(n是指任意数字number) | 删除/剪切光标所在行之后的n行
yy | 复制光标所在行
nyy | 从光标所在行开始, 向下复制n行
p | 从光标所在行开始 , 向下粘贴已经复制或者剪切的内容
P | 从光标所在行开始 , 向上粘贴已经复制或者剪切的内容
u | 还原上一步操作(撤回)
ctrl+r | 对u键的撤回进行反撤回
v | 按v后移动光标会选中指定字符, 然后可以实现复制 , 粘贴等操作









## 扩展
vim的特殊用法 http://www.apelearn.com/bbs/thread-9334-1-1.html 

vim常用快捷键总结 http://www.apelearn.com/bbs/thread-407-1-1.html 

vim快速删除一段字符 http://www.apelearn.com/bbs/thread-842-1-1.html 

vim乱码 http://www.apelearn.com/bbs/thread-6753-1-1.html 

小键盘问题 http://www.apelearn.com/bbs/thread-7215-1-1.html 

vim加密  http://www.apelearn.com/bbs/thread-7750-1-1.html 




