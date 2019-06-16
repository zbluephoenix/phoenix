---
title: 服务器搭建记录
date: 2019-06-16 15:45:21
category:
- Software
- Server
tags:
---

1. 系统使用`Ubuntu Desktop 18.04LTS`
2. 使用 `Swift` 的 `SwiftNIO` 框架搭建服务器 

<!--more-->

目前是在个人笔记本重新做的 `Ubuntu Desktop 18.04LTS` 系统, 使用路由器构建本地局域网，充当服务器

> 首次尝试安装 `Ubuntu Server 18.04LTS`, 在配置完成之后，重新启动失败。
> 还未寻找原因, 日后抽时间寻找原因

个人笔记本电脑配置

1. AMD A8-4500M APU with Radeon(tm) HD Graphics
2. 内存 6G 一个4G内存条，一个2G内存条
3. 硬盘 700G


## 搭建Swift运行环境

1. [官方安装clang教程](https://swift.org/getting-started/#installing-swift)
	
	```shell
	$ sudo apt-get install clang
	```

2. [下载 Swift 最新版 ToolChain](https://swift.org/download/#releases)

3. 解压Swift ToolChain
	
	```shell
	$ tar -xzf <ToolChain压缩文件名>				## 解压
	$ mv <ToolChain文件夹名> <目的地文件名>		## 移动文件到你想制定的位置，可以忽略这步
	$ echo 'export PATH=<目的地文件名/usr/bin>:"${PATH}"' >> ~/.bashrc		## 将ToolChain的 bin文件夹 添加到环境变量 PATH 中
	$ source ~/.bashrc			## 使新增的 PATH 变量生效
	$ swift --version 			## 查看安装是否成功，成功将显示版本号
	```


## 使用Git管理部署服务器代码

当前个人电脑局域网ip为 `192.168.2.105` 用户名 `jabin`

1. 使用Git创建一个裸仓库

	```shell
	$ cd ~
	$ mkdir demo.git
	$ cd demo.git
	demo.git$ git init --bare
	```

2. 局域网内其他设备通过ssh访问

	```shell
	$ ssh jabin@192.168.2.105:demo.git
	```

3. 本机通过ssh被访问需要先安装 `openssh-server`
	
	```shell
	$ sudo apt-get install openssh-server
	```

4. 其他设备通过ssh访问服务器需要安装 `openssh-client`  Ubuntu默认安装了 `openssh-client`
	
	```shell
	$ sudo apt-get install openssh-client
	```

5. Ubuntu中查看是否安装`ssh`相关服务
	
	```shell
	$ dpkg -l | grep ssh
	```

6. 使用Mac进行开发，提交到服务器上
	
	```shell
	$ git remote add origin jabin@192.168.2.105:demo.git
	$ 省略中间本地 add commit 等步骤
	$ git push
	```

7. 在服务器上将 `demo.git` clone 到服务器中的文件夹中，启动服务

	```shell
	$ cd ~
	$ mkdir serverDemo
	$ cd serverDemo
	serverDemo$ git clone /home/jabin/demo.git
	serverDemo$ swift run 				## swift 运行 package 包命令
	```


## Swift 开启 NIOHTTP1Server Demo

当前个人电脑局域网ip为 `192.168.2.105`

```shell
$ swift run NIOHTTP1Server localhost 8090 /var/www
```

在局域网内其他电脑访问时遇到如下问题

```
Unable to round-trip http request to upstream: dial tcp 192.168.2.105:8090: connect: connection refused
```

重新使用新命令开启服务器

```shell
$ swift run NIOHTTP1Server 192.168.2.105 8090 /var/www
```

局域网内其他电脑可以正常通过 `192.168.2.105` 访问

## Swift Source 构建

1. 目前进行到 [Building Swift](https://github.com/apple/swift) 步骤