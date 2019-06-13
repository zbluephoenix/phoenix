---
title: Swift脚本(基础)
date: 2019-06-13 15:58:49
category:
- Software
- Swift
tags:
---

使用Swift实现脚本

<!--more-->

## 安装swift需要的环境

具体安装教程 [在这里](https://swift.org/getting-started/#installing-swift) 
支持 macOS , Linux

## 创建第一个HelloWorld

新建一个 `HelloWorld.swift` 文件

```
$ touch HelloWorld.swift
```

编辑该文件， 输出HelloWorld

```
print("Hello World")
```

通过 swift 命令， 运行该文件

```
$ swift HelloWorld.swift

Hello World
```


## 添加Shebang符号(#!) 解释器指令

编辑 `HelloWorld.swift` 文件

```
#!/usr/bin/env swift

print("Hello World")
```

给文件添加执行权限

```
$ chmod +x HelloWorld.swift
```

直接执行该文件

```
$ ./HelloWorld.swift

Hello World
```


## 给脚本添加需要使用的三方库

执行如下命令，查看需要的帮助

```
$ swift --help
```

此次举例 使用 三方 Framework

根据`help` 选择 `-F`参数， 参数 `-F`后接 包含framework的路径

假设当前目录存在 `Unit.framework`  

`./` 代表当前目录，这里使用了相对路径

添加 framework 的搜索路径
```
$ swift -F ./ HelloWorld.swift
```

或者修改文件顶部的 Shebang

```
#!/usr/bin/env swift -F ./
```

在 `HelloWorld.swift` 中添加 `import Unit` 就可以正常使用 `Unit` 模块了

### -F 后参数的注意事项

1. -F 后最好拼接绝对路径
2. 添加多个路径的方法是 e.g. `$ swift -F ./ -F ./Demo/ HelloWorld.swift` 

