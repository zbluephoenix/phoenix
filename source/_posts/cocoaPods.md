---
title: cocoaPods安装步骤与问题
date: 2016-08-02 15:34:34
category: 
- Software
- iOS
tags: 
- gem
- cocoaPods
---

本人在安装cocoaPods过程中遇到很多问题，只能各种查资料，逐一解决，为了方便以后自己再次安装，同时也为了方便大家，特意写下此文章

<!--more-->

## 安装cocoaPods
### 查看gem源

```
$ gem sources -l
```
如果你的gem源为`https://gems.ruby-china.com`那就不用换源，否则先给gem换源

```
$ gem sources --remove https://rubygems.org/
$ gem sources -a https://gems.ruby-china.com
$ gem sources -l
```
### 安装

```
$ sudo gem install cocoapods
```
备注：苹果系统升级 OS X EL Capitan 后改为

```
$ sudo gem install -n /usr/local/bin cocoapods
```

我的电脑在执行上述步骤时，gem还不是最新的版本，所以最后一行有一个错误，我不确定这个错误最后是否影响cocoapods的安装，但是我还是对它更新了。

```
$ sudo gem update --system
```

**更新完成之后在重新执行cocoaPods安装命令**

## 配置cocoaPods的环境
### 配置cocoaPods

```
$ pod setup --verbose 
```
第一次配置此环境需要下载大概40MB的文件，而且是在github上下载。速度非常慢，如果你的网速非常好，你可以等待它安装完

我本人在安装时网速并不好，所以最后我选择在国内镜像源`https://git.coding.net/hging/Specs.git`处下载。


在这里下载完成之后，将文件名改为**master**复制到`/User/mroid(本机的用户名)/.cocoapods/repos`文件夹内，这个文件夹是pod本地库文件（我本人是这么理解的）。然后执行**`pod setup`**

**`pod setup`** 后面带有**`--verbose`**是为了显示配置过程中的详细信息

## 更新库文件
以后在更新库文件的时候执行此命令

```
$ pod repo update --verbose
```

## 在工程中添加第三方
使用终端打开工程根目录
在工程根目录下新建Podfile文件（注意：此文件没有后缀名）

```
$ vim Podfile
```
Podfile文件中的内容：

```
platform :ios, ‘8.0’

target 'XPPuzzle' do
pod 'FMDB'
pod 'CTAssetsPickerController'
end
```
target 后面接的是你的工程的名字
在文本编辑界面按`ESC`切换为末行模式输入`:wq`按下回车返回终端命令行输入

```
$ pod install
```

更新工程内的第三方文件时输入

```
$ pod update
```
> 在执行`pod install`命令或`pod update`时除了会在工程内安装和更新第三方，还有可能会更新本地库文件，所以为了提升下载速度，可以用`pod install --no-repo-update`或`pod update --no-repo-update`命令代替以上两个命令

## 相关资料
> [安装cocoaPods准备工作：安装／更新ruby环境教程](zbluephoenix.cn/2016/08/02/ruby/)