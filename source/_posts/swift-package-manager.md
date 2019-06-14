---
title: Swift Package Manager
date: 2019-06-14 13:57:05
category:
- Software
- Swift
tags:
---

整理使用中遇到的问题

Swift官方提供的[包管理器](https://swift.org/package-manager/)

<!--more-->

## 使用Swift包管理器创建 package 注意事项

### 命名问题

1. Package 的每一个 target (也就是在使用时 import 的 model 的名字) 的名字，一定要保证独一无二。最好的办法按照OC的命名规范添加前缀 `JAZ` `Rx`
2. 如果出现`Model`名字重复就有可能出现问题
    1. 假设存在 ModelA , ModelB
    2. ModelA 包含 struct Hello
    3. ModelB 包含 struct ModelA 与 struct Hello
    4. ModelC 同时使用 ModleA 与 ModelB 
    5. 此时 ModelA 中的 Hello 结构体将无法使用