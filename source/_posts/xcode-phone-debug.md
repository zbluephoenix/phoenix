---
title: Xcode真机调试 免证书
date: 2016-07-27 18:14:33
category: iOS
tags: 调试
---

苹果的真机调试一直是一件麻烦的事情，不过现在苹果官方允许不用开发者帐号就可以真机调试，不过需要Xcode7版本以上，OSX10.11以上

<!--more-->

1. 首先你需要有一个Apple ID（具体你要怎么拥有这个ID 这里我就不多说了）
2. 然后打开Xcode 在菜单栏Xcode中单击Preferences->Accounts
![](http://o9pu9elcp.bkt.clouddn.com/caidanlan.png)
3. 单击界面左下角＋ 然后输入你的Apple ID 然后等待你的ID添加完成，稍等一会在界面右端Team中会有你的名字 Role处显示Free
![](http://o9pu9elcp.bkt.clouddn.com/Accounts.png)

-----
<center>至此你的Apple ID就可以进行真机调试了</center>

<center>下面开始介绍在工程内该怎么设置</center>

-----

1. 打开工程，单击工程名字；
2. 然后在General中的Identity项中修改Team
3. 将Team选择你刚刚添加的Apple ID
4. 然后在Team下方单击Fix Issue 稍等片刻等待Team下的警告消失即可
![](http://o9pu9elcp.bkt.clouddn.com/project.png)
5. 将你的手机通过数据线连接电脑 将调试模拟器改为你的手机 Command+R运行程序

----
<center><font color = red>**现在就是见证奇迹的时刻**</font></center>

<center>你的程序是不是已经在你的手机上了
不过此刻的程序还不能运行</center>

<center>你需要进行以下的步骤</center>

----

1. 手机进入设置->通用->设备管理
<center><img src="http://o9pu9elcp.bkt.clouddn.com/phone.png" width = 300/></center>
2. 进入设备管理之后有你的Apple ID的帐号 进入帐号信任此开发者 <center><img src="http://o9pu9elcp.bkt.clouddn.com/app.png" width = 300/></center>
<center>我手机已经信任过了，所以截取的图片是成功后的样式</center>
3. 现在你的程序就可以在你的手机上运行了





