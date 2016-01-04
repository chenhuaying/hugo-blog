---
date: 2016-01-04T12:40:49+08:00
title: "Foundation学习笔记"
categories:
- 技术文章
tags: [objective-c]

---

## Foundation
Foundation为objective-c提供了数字、字符串、数组，字典等数据结构，能像其它高级语言Python/PHP/Go那样操作字符串、数组、字典与集合等。如果不知道怎么写，可以先想想python怎么做的，然后再看看Foundation对应的API。

### 数字
NSNumber，需要`#import <Foundation/NSValue.h>`

封装了对数字的操作，包括整数，浮点数

### 字符串
NSString，需要`#import <Foundation/NSString.h>`

封装字符串操作，有很多类似Python等高级语言里的操作都有

* 不可变字符串，@"string"生成的是不可变字符串NSConstantString
* 可变字符串，NSMutableString

NSMutableString与NSConstantString都是NSString类的子类

### 数组
NSArray，需要`#import <Foundation/NSArray.h>`

* NSArray是不可变数组
* NSMutableArray是可变数组，是NSArray的子类

数组基本操作

#### 1. 初始化不可变数组    
```
NSArray *monthName = [NSArray arrayWithObjects:
  @"January", @"February", @"March", @"April",
	@"May", @"June", @"July", @"August", @"September",
	@"October", @"November", @"December", nil];

```
最后一个必须指定为nil，但它不会存储在数组中

#### 2. 可变数组初始化    
```
NSMutableArray *myMutableArray = [[NSMutableArray alloc] init]
```
通过alloc来分配数组可以获得数组的所有权，如果用array方法分配数组，只有它的使用权，它的所有权属于NSMutableArray**有待于测试它的真实含义**

#### 3. 枚举
```
for ( T *v in myArray ) {
	...
}
```

### 字典
NSDictionary，需要`#import <Foundation/NSDictionary.h>`，字典的Key可以是任何对象，但必须是单值，Value可以是任何对象，不能是nil

NSMutableDictionary是NSDictionary的子类

枚举
```
for (T *key in myDict) {
  [myDict objectForKey: key]
}
```

### 集合
NSSet, 需要`#import <Foundation/NSSet.h>`，支持交集与并集的操作

NSMutableSet是NSSet的子类

枚举
```
for (T *e in mySet) {
  [e method]
}
```

还有一种NScountedSet，支持一个集合中有多个值，增加一个count+1，删除一个count-1，如果count为零则删除对象

