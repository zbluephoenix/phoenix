---
title: Bug收集贴
date: 2019-06-12 23:44:29
category:
- Bug
- General
tags:
---

遇到的无明确分类的问题

<!--more-->


## 已经安装node 提示 node: no such file or directory

原因 node 执行文件 不在环境变量中

解决方案

找到系统中的node执行文件安装的位置
1. 将此执行文件链接到现有的环境变量目录下 
创建快捷方式命令
```
$ ln -s /usr/xxxx/xxx/node /usr/xx/xxx/node
```

2. 将此目录添加到环境变量 `PATH` 中
