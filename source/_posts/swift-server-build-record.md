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

2. 使用Mac进行开发，提交到服务器上
	
    冒号后拼接的是jabin用户目录下的 git 仓库的路径，如果 用户目录下存在store文件夹，store文件中存在demo.git，冒号后拼接 `store/demo.git`

	```shell
	$ git remote add origin jabin@192.168.2.105:demo.git
	$ 省略中间本地 add commit 等步骤
	$ git push
	```

3. 在服务器上将 `demo.git` clone 到服务器中的文件夹中，启动服务

	```shell
	$ cd ~
	$ mkdir serverDemo
	$ cd serverDemo
	serverDemo$ git clone /home/jabin/demo.git
	serverDemo$ swift run 				## swift 运行 package 包命令
	```

## 通过SSH访问服务器

1. Ubuntu中查看是否安装`ssh`相关服务
    
    ```shell
    $ dpkg -l | grep ssh
    ```

2. 本机通过ssh被访问需要先安装 `openssh-server`
    
    ```shell
    $ sudo apt-get install openssh-server
    ```

3. 其他设备通过ssh访问服务器需要安装 `openssh-client`  Ubuntu默认安装了 `openssh-client`
    
    ```shell
    $ sudo apt-get install openssh-client
    ```

4. Ubuntu查看是否开启ssh相关服务
    ```shell
    $ ps -e | grep ssh
    1xxx ?     00:00:00  sshd       ## 表示ssh服务开启

    ## 开启服务 / 关闭 / 重启
    $ sudo /etc/init.d/ssh start
    $ sudo /etc/init.d/ssh stop 
    $ sudo /etc/init.d/ssh restart
    ```

5. 局域网内其他设备通过ssh访问 
    
    如果没有将公钥添加到服务器 将需要输入jabin用户的登录密码，进行登录
    如果已经在服务器添加过公钥，则可以直接访问服务器

    ```shell
    $ ssh jabin@192.168.2.105   ## 默认访问端口22 ，使用-p 参数修改端口
    ```

6. 向服务器添加公钥的方法
    
    如果当前设备已经创建过**一个**ssh密钥，则使用以下命令，可以将公钥添加到服务器，如果存在**多个**ssh密钥文件，通过`-i`参数进行选择

    ```shell
    $ ssh-copy-id jabin@192.168.2.105
    ```

    如果当前设备没有ssh密钥， 使用以下命令创建

    ```shell
    $ ssh-keygen -t rsa
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