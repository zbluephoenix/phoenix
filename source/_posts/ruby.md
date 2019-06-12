---
title: 安装cocoaPods准备工作：安装／更新ruby环境
date: 2016-08-02 08:44:38
category: 
- Software
- iOS
tags: 
- ruby
- rvm
- cocoaPods
---
本人在安装cocoaPods的过程中遇到了大量问题，每次问题都需要花费一定的时间查找资料，这次安装成功之后，我将我遇到的错误和解决办法，进行了一个汇总，写出了以下的说明：

<!--more-->

有时候在安装cocoaPods时会出现如下错误提示

```
ERROR:  Error installing cocoapods: activesupport requires Ruby version >= 2.2.2
```
这是因为你的ruby版本较低，mac本身是自带ruby，但是自带的版本较低。

我以在mac osx 10.11下更新ruby版本进行为例

我是通过rvm对ruby进行升级的（rvm是一个ruby版本管理和gem库管理的命令行工具）

## 安装rvm
### 首先安装rvm

```
$ curl -L get.rvm.io | bash -s stable
```
	

### 加载文件测试是否正常安装（按提示操作）同时查看rvm版本

```
$ source ~/.bashrc  
$ source ~/.bash_profile  
$ source ~/.profile
$ rvm -v
```

### 我查到的资料在rvm -v之后提示如果有如下的错误，请reload rvm

```
A RVM version 1.27.0 (latest) is installed yet 1.25.23 (stable) is loaded.
Please do one of the following:
* 'rvm reload'
* open a new shell
* 'echo rvm_auto_reload_flag=1 >> ~/.rvmrc' # for auto reload with msg.
* 'echo rvm_auto_reload_flag=2 >> ~/.rvmrc' # for silent auto reload.
```
但是我并没有遇到此错误，直接显示为最后一行
  	
> 至此rvm安装完成，现在要通过rvm对ruby进行升级

## 升级ruby
### 查看当前的ruby版本，获取rvm可以查到的版本列表

```
$ ruby -v   
$ rvm list known
```

### 选择一个高于2.2.2的版本进行安装，我这里安装的2.3
	
```
$ rvm install 2.3
```
	
我查找的资料在这步介绍说可能有两种错误
> 错误1: 在安装ruby的时候, 可能会如下报错
	
>Updating 	system[YourMacName] password required 	for ‘port 	-dv self update’
	
解决方法：
	
```
$ sudo port self update
```
> 错误2:需要安装Homebrew
	
```
Error running 'requirements_osx_port_libs_install curl-ca-bundle automake libtool libyaml libffi libksba',
showing last 15 lines of /Users/acewill/.rvm/log/1468253599_ruby-2.3.0/package_install_curl-ca-bundle_automake_libtool_libyaml_libffi_libksba.log
```
解决方法：
	
```
ruby -e "$(curl -fsSL https://raw.githubusercontent.com/Homebrew/install/master/install)"
```
以上两种错误我都没有遇到，我在安装的时候的确需要安装Homebrew，但是是在安装过程中直接提示我选择Homebrew的本地安装位置，我选择的是`/usr/local/Homebrew`然后在安装到`Updating system.....`这个步骤的时候，我第一次安装到这里失败了，后来我用VPN重新安装成功的。

### 安装rails

```
$ gem install rails
```

### 卸载ruby方法 
获取ruby已安装版本列表，然后卸载ruby
	
```
$ rvm list
$ rvm remove 2.3
```
## 相关资料
> [cocoaPods安装步骤与问题](https://zbluephoenix.cn/2016/08/02/cocoaPods/)

## 参考资料
> [cocoapods:安装/更新Ruby环境教程](http://blog.csdn.net/wangyanchang21/article/details/51885383)