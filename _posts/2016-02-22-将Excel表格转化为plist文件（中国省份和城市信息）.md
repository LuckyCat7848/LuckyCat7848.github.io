---
layout: post
title:  将Excel表格转化为plist文件（中国省份和城市信息）
date:   2016-02-22 13:45:23 +0800
categories: 技术
tag: Object-C
---


* TOC
{:toc}





将图1的Excel表格转换成图2的.plist文件，两张图如下：

图1：

![]({{ '/source/将Excel表格转化为plist文件（中国省份和城市信息）/1479312936089.png'}})

图2：

![]({{ '/source/将Excel表格转化为plist文件（中国省份和城市信息）/1479312949609.png'}})

-------------------------------------

**1.** 先把Excel表格另存为.csv格式：

![]({{ '/source/将Excel表格转化为plist文件（中国省份和城市信息）/1479312967213.png'}})

**2.** 将.csv文件内容复制粘贴到新的.txt文件中;

**3.** 将.txt文件拖拽到工程中，用代码读取.txt文件内容写入到.plist文件。

``` python
NSString * path = [[NSBundle mainBundle] pathForResource:@"ProvincesAndCities" ofType:@"txt"];
    NSString * contents = [[NSString alloc] initWithContentsOfFile:path encoding:NSUTF8StringEncoding error:nil];
    // 获取到excel中n行的数组
    NSArray * contentsArray = [contents componentsSeparatedByCharactersInSet:[NSCharacterSet newlineCharacterSet]];

    NSString *savePath = [NSHomeDirectory() stringByAppendingPathComponent:@"Documents/ProvincesAndCities.plist"] ;

    NSMutableArray *allArr = [NSMutableArray array];
    for (int i = 0; i < contentsArray.count; i++)
    {
        NSString * lineStr = contentsArray[i];
        // 将一行数据转为数组格式，对其遍历，获取单个格里的内容
        NSArray * lineArr = [lineStr componentsSeparatedByCharactersInSet:[NSCharacterSet controlCharacterSet]];
        for (int j = 0; j < lineArr.count; j++)
        {
            NSString * oneStr = lineArr[j];
            if (![NSString strIsNilOrEmpty:oneStr]) {
                NSMutableDictionary * rowDic = nil;
                if (i == 0) {
                    // 第一行是省份名称
                    rowDic = [[NSMutableDictionary alloc] init];
                    [rowDic setObject:lineArr[j] forKey:@"province"];
                    [allArr addObject:rowDic];
                }
                else {
                    // 城市按每列存在数组中，再保存到每列信息里
                    rowDic = [NSMutableDictionary dictionaryWithDictionary:allArr[j]];
                    NSMutableArray * citiesArr = [NSMutableArray arrayWithArray:rowDic[@"cities"]];
                    if (![citiesArr containsObject:oneStr]) {
                        [citiesArr addObject:oneStr];
                    }
                    [rowDic setObject:citiesArr forKey:@"cities"];
                    [allArr replaceObjectAtIndex:j withObject:rowDic];
                }
            }
        }
    }
    // 写入到plist文件
    [allArr writeToFile:savePath atomically:YES];
```

**4.** 最后，就可以直接在工程里删除.txt，使用.plist文件了。

