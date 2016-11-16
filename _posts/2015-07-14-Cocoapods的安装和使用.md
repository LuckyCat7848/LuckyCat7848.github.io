---
layout: post
title:  Cocoapods的安装和使用
date:   2015-07-14 19:24:22 +0800
categories: 技术
tag: 工具
---


* TOC
{:toc}




>**RubyGems**淘宝镜像已停更，本人现在使用**Ruby China**提供的镜像。

---------------------------------------

**Step 1.** 查询现在使用的淘宝镜像：

![]({{ '/source/Cocoapods的安装和使用/Step1-01.png'}})

**Step 2.** 删除淘宝镜像，添加Ruby China镜像：

![]({{ '/source/Cocoapods的安装和使用/Step2-01.png'}})

 >**失败原因：**苹果系统升级 OS X EL Capitan后命令加上**sudo**（具体原因不懂）。
 
 **加上sudo后重新操作**：
 
 ![]({{ '/source/Cocoapods的安装和使用/Step2-02.png'}})
 
可以看到当前已经是Ruby China镜像。

**Step 3.** 使用**sudo gem update --system**更新RubyGems。

**Step 4.** 查看版本信息：

![]({{ '/source/Cocoapods的安装和使用/Step4-01.png'}})

**Step 5.** 安装CocoaPods

- **$ sudo gem install cocoapods **

>**备注：**苹果系统升级 OS X EL Capitan 后改为 **sudo gem install -n /usr/local/bin cocoapods**（我的是以前安装好了的，这次主要是更新Ruby China源，无安装Coapods截图）

- **$ pod setup**

**Step 6.** 新建工程，并在终端用cd指令到文件夹内：

![]({{ '/source/Cocoapods的安装和使用/Step6-01.png'}})

**Step 7.** 新建文件Podfile：

![]({{ '/source/Cocoapods的安装和使用/Step7-01.png'}})

**Step 8.** 编辑Podfile文件内容：

**低版本：**

![]({{ '/source/Cocoapods的安装和使用/Step8-01.png'}})

**高版本的cocoapods的Podfile文件必须添加target：**

![]({{ '/source/Cocoapods的安装和使用/Step8-02.png'}})

>**提示：**编辑Podfile文件，按a或i可编辑，esc 退出编辑，“”：wq”可保存退出，“q!”直接退出不保存。

**Step 9.** 安装：

![]({{ '/source/Cocoapods的安装和使用/Step9-01.png'}})

**Step 10.** 到**.xcodeproj**文件下查看，多了个**.xcworkspace**，之后使用这个文件打开工程，即可看到已导入的库文件。
