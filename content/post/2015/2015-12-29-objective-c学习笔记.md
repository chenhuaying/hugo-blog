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
       memberDeclarations;  // 成员变量声明
   }
   methodDeclarations;      // 成员函数声明
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
        调用时采用如下形式:     
				`[instance setTo: 1 over: 3]`
   7. 不带参数名的方法    
      `-(void) set: (int) n : (int) d`    
      调用形式:     
			`[instance set: 1 : 3]`
   8. @class声明一个类型，在接下来的作用域中就可以直接使用@class声明过的类:
      `@class ClassName`    
      可以用`#import <class-interface.h>`来代替，但用@class效率更高
      * 要使用class中的方法，还是得用**import**方式

### 定义: @implement
1. 定义格式:
   ```
   @implementation NewClass
       methodDefinitions;  // 成员函数定义
   @end
   ```
   @implementation NewClass: NSObject实现的父类是可选的，通常不用显示的写出，声明必须要有

2. 属性@synthesize    
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

### 初始化
分配内配后通常要调用初始化方法，一定要正确的初始化**继承**而来的成员变量，首先调用父类的初始化方法，再初始化自己的成员变量
```
-(MyClass *) initWith: (int) a : (int) b
{
	self = [super init];
	if (self)
	  [self setA: a withB: b];
	return self;
}
```

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
类对象通常是由class方法产生的对象，如：    
`[ClassName/(an instance of ClassName) class]`     
都将得到一个ClassName类对象

有如下操作:

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

## 作用域
### 作用域限定指令
* @private，只能在该类的方法中使用，其子类也不能使用
* @protected，能在该类及其子类中使用，**默认情况**；C++类的默认情况则是private
* @public，能在声明后的任意位置访问
* @package，包内访问

### C类语言作用域
与C类语言相似的作用域，意义也相同
1. 外部变量，extern
2. 静态变量，static

## 存储类说明符
1. auto，与C++11的auto不同，这里表示的是自动变量，通常指的是局部变量，在一个函数或一个代码块中的变量，可以省略
2. const，常量
3. volatile，防止编译器优化

## 数据类型
### 枚举类型
与C语言类似的语法，第一个赋值为0
```
enum flag {
	on,
	off
};
```

也可以给他们赋值
```
enum weekDay {
	monday = 1,
	tuesday, wednesday, thursday, friday, saturday, sunday
};
```

### 别名
typedef，相当于给一个类型取了个别名    
`typedef typeName newName`

## 分类与协议
### 分类
用于扩展已有的类型，而无需访问该类的源代码，也就是说可以扩展以二进制形式发布的库！就算是可以访问源代码的类，通过这种方式扩展更安全。

其声明和定义的语法与类的定义非常相似，圆括号中的是分类名。

声明的格式：
```
@interface ClassName (categoryName)
-(return type) method: (parameter type) parameter;
...
@end
```
声明中不能指定类的父类，否则编译器会报错

定义的格式：
```
@implementation ClassName (categoryName)
    // code for category  method implementation
@end
```
分类中可以重载类中的方法，会影响到该类及其子类，通过**分类**重载类方法要**谨慎**使用;

不必实现分类中的所有方法，可以先定义所有方法，以后再慢慢实现。

### 协议
协议定义一组方法，遵循该协议的类需要实现这个协议中的**所有**方法，**@optional**指令下的方法**除外**

#### 1. 声明
```
@protocol ProtocolName
// methods declare
@optional
// optional methods declare
@end
```
@optional表示可选协议，遵循该协议的类不是必须实现可选方法；与之对应的是**@required**

#### 2. 采用协议/遵循协议    
要让某个类遵循一个协议，在@interface声明时，用<...>列出协议的名称，多个协议用**,**分隔
```
@interface ClassName: BaseClass <ProtocaoName, ProtocolName2, ...>
{
    // members declare
}
// methods declare
@end 
```

#### 3. 分类采用协议
    @interface ClassName (categoryName) <ProtocolName1, ProtocolName2, ...>

#### 4. 协议扩展
协议采用或遵循协议

    @protocol ExtendProtocol <ProtocolName>

#### 5. 非正式协议
非正式协议或者叫**抽象协议**，其实是一个分类，列出一组方法。如果一个类采用正式协议，必须实现协议下的所有方法。如果采用的是非正式协议这不用实现协议下的所有方法。
```
@interface ClassName (categoryName)
// methods declare
@end
```

#### 6. 通知编译器做类型检查
在类名称之后的尖括号中添加协议名称，如下：    

    id <ProtocolName1, ProtocolName2, ...> myObject;
在给myObject赋值时，会进行类型检查，如果该值不遵循指定的协议，编译器会报警

#### 7. 检查对象是否遵循协议
```
id myObject;
...
if ([myObject conformsToProtocol: @protocol (ProtocolName)]) {
    // do operation
}
```

#### 8. 聚合或者组合
objective-c支持组合的方式扩展，**不支持**嵌入扩展

## C语言特性
C语言特性要用C的方式来操作，不要用objective-c的方式操作
### 宏扩展
与C的宏相同，支持条件编译，定义宏名等，按照C语言的方式理解即可
### 数组
### 结构体
### 联合
### 函数
### 其它语言特性

## 总结
objective-c是在C语言的基础上的改进，它们之间有联系，又有区别。objective-c增加了对面向对象编程技术的支持，面向对象部分与C++有些概念相似，但比C++简洁。语言级别的学习先到此告一段落，接下来是Foundation框架的学习，iOS编程的基础。
