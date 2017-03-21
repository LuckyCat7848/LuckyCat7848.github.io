---
layout: post
title:  iOS基础：@synthesize和@dynamic区别
date:   2015-02-10 10:14:39 +0800
categories: 技术
tag: Object-C
---


* TOC
{:toc}




在声明property属性后，有2种实现选择：

synthesize
=====

编译器期间，让编译器自动生成getter/setter方法。
当有自定义的存或取方法时，自定义会屏蔽自动生成该方法

dynamic
=====

告诉编译器，不自动生成getter/setter方法，避免编译期间产生警告，然后由自己实现存取方法。

或存取方法在运行时动态创建绑定：主要使用在CoreData的实现NSManagedObject子类时使用，**CoreData会在运行时动态为所有Category中的属性生成实现代码。**
