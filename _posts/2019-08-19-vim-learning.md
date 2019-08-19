---
layout:     post
title:      "vim学习"
date:       2019-08-19 18:38:00
author:     "Hux"
header-img: "img/post-bg-miui6.jpg"
tags:
    - vim
---

#
<ESC> key: 进入Normal mode
```bash
//exit and discard changes
:q! <ENTER>

//exit and save changes
:wq! <ENTER>



//insert text before cursor
i

//append text after line
a
```

## d
d motion
d: delete operator
motion: what the operator will operate on

  A short list of motions:
    w - delete word
    e - delete to the end of the current word
    $ - delete to the end of the line

> Typing a number with an operator repeats it many times.
## number + motion 
在motion前面加数字，表示motion重复number次
```bash
2w //将cursor向前移动2个词
3e //move the cursor to the end of the third word forward
0 // move to the start of the line
```

## d+number+motion 
d3w: 向前删除3个词

## 删除整行
```bash
dd //删除整行
2dd //删除两行
```

### 参考资料
1. 命令行直接输入vimtutor

