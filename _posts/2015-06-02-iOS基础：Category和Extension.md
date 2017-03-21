---
layout: post
title:  iOS基础：Category和Extension
date:   2015-06-02 14:05:53 +0800
categories: 技术
tag: Object-C
---


* TOC
{:toc}


-------------

一、什么是Category
======

Category模式用于向已经存在的类添加方法从而达到扩展已有类的目的，在很多情形下Category也是比创建子类更优的选择。新添加的方法同样也会被被扩展的类的所有子类自动继承。当知道已有类中某个方法有BUG，但是这个类是以库的形式存在的，我们无法直接修改源代码的时候，Category也可以用于替代这个已有类中某个方法的实体，从而达到修复BUG的目的。然而却没有什么便捷的途径可以去调用已有类中原有的那个被替换掉方法实体了。需要注意的是，当准备有Category来替换某一个方法的时候，一定要保证实现原来方法的所有功能，否则这种替代就是没有意义而且会引起新的BUG。和子类不同的是，Category不能用于向被扩展类添加实例变量。Category通常作为一种组织框架代码的工具来使用。

二、Category的用途
====

1. 在不创建继承类的情况下实现对已有类的扩展；

2. 简化类的开发工作（当一个类需要多个程序员协同发的时候，Category可以将同一个类根据用途分别放在不同的源文件中，从而便于程序员独立开发相应的方法集合）；

3. 将常用的相关的方法分组；

4. 在没有源代码的情况下可以用来修复BUG。


三、Category的用法
====

在Obj-C中，声明某一个已有类的Category扩展的方法如下：

``` bash
interface ClassName (CategoryName)
- (void)methodName1;
- (void)methodName2;
end
```

上面的声明通常是在.h文件中，然后我们在.m文件中实现这些方法：
 
``` bash
implementation ClassName (CategoryName)
- (void)methodName1;
- (void)methodName2;
end
```
 
我们创建一个iOS Single View Applciation名为CategoryExample。然后为创建一个NSString类的category扩展。**File->New->File然后选择 Cocoa Touch Objective-C->category**，命名为ReverseNSString。系统会自动生成一个固定格式ClassName+CategoryName的.h和.m文件。


3.1 声明Category
--------

打开NSString+ReverseNSString.h文件，在里面添加如下代码：

``` bash
import <Foundation/Foundation.h>  
interface NSString (ReverseNSString)  
+ (NSString*) reverseString:(NSString*)strSrc;  
end  
```

3.2 实现Category
---------

NSString+ReverseNSString.m文件中实现reverseString方法：

``` bash
import"NSString+ReverseNSString.h"  
implementation NSString (ReverseNSString)  
+ (NSString*)reverseString:(NSString*)strSrc;  
{  
    NSMutableString *reversedString =[[NSMutableString alloc]init];  
    NSInteger charIndex = [strSrc length];  
    while (charIndex > 0) {  
        charIndex--;  
        NSRange subStrRange =NSMakeRange(charIndex, 1);  
        [reversedString appendString:[strSrcsubstringWithRange:subStrRange]];  
    }  
    return reversedString;  
}  
@end  
```

剩下的工作就是验证我们的Category了，在view中添加一个按钮ReverseString,并设置相应的action方法为reverseString.在view上再添加一个label，命名为myString，默认值是”HelloCategory Design Pattern!”。点击按钮反转这个字符串。主要代码如下：

``` bash
-(IBAction)reverseString:(id)sender {  
    NSString *test = [NSStringreverseString:_myString.text];  
    _myString.text = test;     
}  
```

四、代码组织
===

Category用于大型类有效分解。通常一个大型类的方法可以根据某种逻辑或是相关性分解为不同的组，一个类的代码量越大，将这个类分解到不同的文件中就显得越有用，每个文件中分别是这个类的某些相关方法的集合。

当有多个开发者共同完成一个项目时，每个人所承担的是单独的cagegory的开发和维护。这样就版本控制就更加简单了，因为开发人员之间的工作冲突更少了。

五、Category VS添加子类
====

并没有什么界限分明的判定标准来作为何时用Category何时用添加子类的方法的指导。但是有以下几个指导性的建议：

1. 如果需要添加一个新的变量，则需添加子类；

2. 如果只是添加一个新的方法，用Category是比较好的选择；

3. 在分类中增加的方法，会被子类所继承，而且在运行时跟其他方法没有区别；

4. 一般不要在分类中覆盖现有类中的方法。


六、Category 不添加成员变量
===

**@property 声明的属性只会自动生成get，set方法，并不能生成下划线的成员属性。**

七、给Category添加属性
====

OC的分类允许给分类添加属性，但不会自动生成getter、setter方法。解决方案是通过运行时建立关联引用。接下来以添加一个这样的属性为例：

``` bash
@property (nonatomic, copy) NSString *str;
```

7.1 引入运行时头文件
------

```bash
import <objc/runtime.h>
```

7.2 在匿名分类（扩展Extension）或者头文件中添加属性
---------

区别是：匿名分类中添加的是私有属性，只在本类中可以使用，类的实例中不可以使用。头文件中添加的在类的实例中也可以使用。

分类的头文件：

``` bash
@interface ClassName (CategoryName)
//我要添加一个实例也可以访问的变量所以就写在这里了
@property (nonatomic, strong) NSString *str;
@end
```

匿名分类（扩展Extension）：

``` bash
@interface ClassName ()
@end
```

7.3 在实现里面写要添加属性的getter、setter方法
-------------

``` bash
@implementation ClassName (CategoryName) 

-(void)setStr:(NSString *)str  
{  
    objc_setAssociatedObject(self, &strKey, str, OBJC_ASSOCIATION_COPY);  
}  

-(NSString *)str  
{  
    return objc_getAssociatedObject(self, &strKey);  
}
@end
```

在setStr:方法中使用了一个objc_setAssociatedObject的方法，这个方法有四个参数，分别是：源对象，关联时的用来标记是哪一个属性的key（因为你可能要添加很多属性），关联的对象和一个关联策略。

用来标记是哪一个属性的key常见有三种写法，但代码效果是一样的，如下：

// 利用静态变量地址唯一不变的特性

``` bash
1、static void *strKey = &strKey;
2、static NSString *strKey = @"strKey"; 
3、static char strKey;
```

关联策略是个枚举值，解释如下：

``` bash
enum {
    OBJC_ASSOCIATION_ASSIGN = 0, //关联对象的属性是弱引用 

    OBJC_ASSOCIATION_RETAIN_NONATOMIC = 1, //关联对象的属性是强引用并且关联对象不使用原子性

    OBJC_ASSOCIATION_COPY_NONATOMIC = 3, //关联对象的属性是copy并且关联对象不使用原子性

    OBJC_ASSOCIATION_RETAIN = 01401, //关联对象的属性是copy并且关联对象使用原子性

    OBJC_ASSOCIATION_COPY = 01403 //关联对象的属性是copy并且关联对象使用原子性
};
```

7.4 完成后的整体代码如下
---------

// 分类的头文件 .h

``` bash
@interface ClassName (CategoryName)
@property (nonatomic, strong) NSString *str;
@end
```

// 实现文件 .m

``` bash
import "ClassName + CategoryName.h"
import <objc/runtime.h>

static void *strKey = &strKey;

@implementation ClassName (CategoryName) 
-(void)setStr:(NSString *)str  
{  
    objc_setAssociatedObject(self, & strKey, str, OBJC_ASSOCIATION_COPY);  
}  

-(NSString *)str  
{  
    return objc_getAssociatedObject(self, &strKey);  
}
@end
```

八、延展（Extension）
===

1. 类的延展就如同时“匿名”的分类，延展中声明的方法在类本身的@implementation和它对应的@end之间实现；

2. 类又是需要方法只有自己所见，我们可以通过延展的方式定义类的私有方法。

![]({{ '/source/iOS基础：Category和Extension/01.png'}})




