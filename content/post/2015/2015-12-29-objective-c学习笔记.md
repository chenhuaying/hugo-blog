---
date: 2015-12-29T14:48:53+08:00
title: "objective-c学习笔记"
categories:
- 技术文章
tags: [objective-c]

---
## 类的定义中的继承与多态
与C++相似，支持继承、重载

### 多态
使用**abstruct** superclass

## 成员变量
下文中__instance__表示某个类的实例，__property__表示类的某个成员变量，setProperty表示该成员变量的setter

1. 获取`[instance property]`
2. 赋值`[instance setProperty: value]`

## 成员函数调用
1. 类方法或成员函数调用
   * objc: `[className/instance method: parameters]`
   * C++: `className::static method`，或者是`object.method`与`pointer->method`
2. new实例化对象
   * objc: `[class new]`
   * c++:  `new class`
   * objective-c中对函数的调用很一致，c++中实例化与函数调用的形式是不一样的
3. 内存分配
   `[ClassName alloc]/[ClassName allocate]`
4. 初始化
   `[ClassName init]`
5. 内存释放
   `[instance release]`

   **alloc, init, release** 从**NSObject**继承而来

## 类定义
类的定义与分为声明和定义两部分
### 声明: @interface
   1. **-** 成员函数，**+** 类方法
   2. 函数返回值
      * 返回值：放在负号后面的括号中**-(int), -(double)**
      * 无返回值：**-(void)**
   3. 方法声明：
   `-(return type) functionName: (parameter type) parameter;`
   4. 声明格式：
```
   @interface NewClass : fatherClass
   {
       memberDeclarations;
   }
   methodDeclarations;
   @end
```

   5. 属性
   @property
   自动生成setter/getter的声明，定义的需要在定义部分使用@synthesize
   `@property int x` 自动生成
   ```
   -(void) setX: (int) a;
   -(int) X();
   ```

   6. 多参数方法
      * 冒号(**:**)连接参数
      * 冒号连接的参数名，是函数签名的一部分，如下面的**over**
        `-(void) setTo: (int) n over: (int) d`
        调用时采用如下形式: `[instance setTo: 1 over: 3]`
   7. 不带参数名的方法
      `-(void) set: (int) n : (int) d`
      调用形式: `[instance set: 1 : 3]`
   8. @class声明一个类型，在接下来的作用域中就可以直接使用@class声明过的类
      `@class ClassName`
      可以用`#import <class-interface.h>`来代替，但用@class效率更高
      * 要使用class中的方法，还是得用**import**方式

### 定义: @implement
1. 定义格式:
   ```
   @implementation NewClass
       methodDefinitions;
   @end
   ```
   @implementation NewClass: NSObject实现的父类是可选的，通常不用显示的写出，声明必须要有

2. 属性
   @synthesize
   `@synthesize x`自动生成
```
    -(void) setX: (int) a
    {
        x = a;
    }

    -(int) X:
    {
       return x;
    }
```
   一行可以定义多个，之间用逗号分隔`@synthesize x, y`

### self关键字
`[self method]`self表示方法的接收者是对象自身
在类的定义中访问成员变量不需要用self

### 点运算符
objective-c 2.0开始可以使用**.**运算符

1. 获取成员变量值: **instance.property**
2. 设置成员变量值: **instance.property = value**

## 基本数据类型
1. 内置数据类型
   * int
   * float
   * double
   * char
2. 字符串
   * 普通字符串: "string"
   * NSString: @"string"
3. 限定
   * long
   * long long
   * short
   * unsigned
   * signed
4. id类型
   * id类型可以存储任意类型的值，有点
   * id类型是**返回值**、**参数**的默认类型
   * 多态、动态绑定

### 格式化输出
NSLog格式化字符

* 整数: %i %o %#o %x %#x %X %#X
* 浮点数: %f %e %g
* 字符: %c
* 字符串: %@

## 表达式
### 运算符
1. 算术运算
+ - * / %
2. 赋值运算
＝ += -= &= |=
3. 位运算
& | ^ ~ >> <<

### 循环
1. for: 与C++相同，支持在语句中声明变量
2. while
3. do
4. break continue

### 条件语句
Boolean: TRUE/FALSE YES/NO

1. if
2. switch
3. ? : 

## 内存管理
### 重载dealloc
1. dealloc函数从NSObject继承而来
2. 释放继承的父类的内存`[super release]`

如下示例：
```
-(void) dealloc
{
  [x release];
  [super dealloc];
}
```

x可以为nil**对象**，这点与C++不同，C++中不能释放null指针所指向的内存地址，也不能用NULL指针调用方法，获取属性等操作，而在objective-c中是**允许**的。

## 多态，动态绑定
### id类型
id类型可以指向存储任何类型的值
```
id f;
Duck *d = [[Duck alloc] init];
Cat *c = [[Cat alloc] init];

f = d
[f say];

f = c
[f say];
```
这个地方有动态语言的特点，不同于C++

### 类对象class-object
类对象通常是由class方法产生的对象，如：`[ClassName/(an instance of ClassName) class]`，都将得到一个ClassName类对象
有如下操作

1. 判断两个对象是否是相同的类
   `if ([obj class] == [obj2 class])`
2. 某个变量是否是是否是属于一个类，即是否是这个类的成员
   `[aValue isMemberof: [ClassName class]]`
3. 一个变量是否是一个类的实例
   `[aInstance isKindOf: [ClassName class]]`
4. 一个类是否是另一个类的子类
   `[ClassName isSubclassOfClass: [BassClassName class]]`

### SEL对象
通常由@selector指令产生，如：`@selector methodName`，将得到一个SEL对象

1. 一个类是否有某一个操作
   `anObject respondsToSelector: @selector methodName `

## 异常处理
1. try-catch
```
   @try {
       statement;
       statement;
       ...;
   }
   @catch (NSException *exception) {
       statement;
       statement;
       ...;
   }
```

2. @throw抛出异常
