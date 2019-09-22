---
layout:     post
title:      "Vim learning"
subtitle:   "vim学习与实践"
date:       2019-09-22 08:50:00
author:     "Xindi"
header-img: "img/post-bg-2015.jpg"
catalog: true
tags:
    - Vim
    - 工具
---


## 前言
学会使用vim的帮助文档进行学习。
进入vim帮助文档：
```
vim
:help
```
![](assets/markdown-img-paste-20190922090625559.png)

查询具体某个命令可以用
```
:help command
```
![](assets/markdown-img-paste-20190922090829841.png)


## 认识vim
```bash
normal mode: 用于浏览文件，Esc
insert mode: 编辑文件，i
command mode: :
visual mode: v
```
提高效率的配置：
```bash
vim ~/.vimrc
#将ESC键映射为两次j键                                      
inoremap jj <Esc>
```

## vim打开多个文件
```bash
vim file1 file2
:ls #列出目前打开的文件
:bn 切换到第n个文件
```

## vim一次性显示多个文件
```bash
vim -on file1 file2 ... filen #左右分屏
vim -On file1 file2 ... filen #上下分屏
```

 ## vim的分屏操作
```bash
ctrl+w  s #上下分割打开的文件
ctrl+w  p #左右分割打开的文件　

```



## 参考文献
[精通vim，此文就够了](https://zhuanlan.zhihu.com/p/68111471)
