---
date: 2016-01-04T19:16:55+08:00
title: "objective-c闭包"
categories:
- 技术文章
tags: [objective-c]

---

Objective-C中的闭包叫做Block，目前阶段只学习使用，不分析原理

Block是C风格的实现方式，可以理解为函数指针，按照函数指针的方式使用闭包。但闭包的定义可以在函数内，比函数灵活。

### block声明
闭包声明符(**^**)，声明符是随便发明的名字，也是一种标识符

    return-type (^ blockName) (parameter type);

#### 不带参数，不带返回值

    void (^ blockName) (void);

#### 多参数
    return-type (^ blockName) (parameter-type1, parameter-type2, ...);

### block定义
**^**表示闭包
```
blockName = ^(parameter-type parameter) {
    ...
};
```
最后有个分号，当然一条完整的语句是需要分号的。

#### 不带参数，不带返回值
```
^{
   ...
}
```
如上也可以赋值，之后的调用形式:
    blockName()

#### 多参数
```
^ return-type (type1 firstValue, type2 secondValue) {
    ...
}
```

### 闭包捕获变量
block能捕获一个代码块内的变量，以**值传递**的方式传递，也就是说闭包内改变捕获的变量的值，在外部是不可见的。并且在闭包定义后改变这个变量的值，在闭包中仍然是改变前的值。
```
int anInteger = 42;

void (^testBlock)(void) = ^{
    NSLog(@"Integer is: %i", anInteger);
};

anInteger = 84;

testBlock();
```
运行结果是:

    Integer is: 42

可以看到在闭包内，捕获的变量没有随着后面的赋值儿发生改变，也就是值传递的方式传递捕获变量。

#### 共享的方式捕获变量
以__block方式声明的变量，以**引用**的方式传递

```
__block int anInteger = 42;

void (^testBlock)(void) = ^{
    NSLog(@"Integer is: %i", anInteger);
};

anInteger = 84;

testBlock();   // 输出: Integer is: 84
```
再看一下闭包改变original变量的值的例子
```
__block int anInteger = 42;

void (^testBlock)(void) = ^{
    NSLog(@"Integer is: %i", anInteger);
    anInteger = 100;
};

testBlock();
NSLog(@"Value of original variable is now: %i", anInteger);    // 输出: Value of original variable is now: 100
```
可以看到，变量的值改变了

### 以参数的形式传递闭包
闭包可以被当作参数来进行传递，如果是在类的成员函数中定义的，可以捕获self，此时要当心[Avoid Strong Reference Cycles when Capturing self](https://developer.apple.com/library/mac/documentation/Cocoa/Conceptual/ProgrammingWithObjectiveC/WorkingwithBlocks/WorkingwithBlocks.html#//apple_ref/doc/uid/TP40011210-CH8-SW16)
问题。

### typedef命名闭包
`typedef return-type (^blockName)(parameter-type1, parameter-type2, ...);`


### 参考
[Programming with Objective-C](https://developer.apple.com/library/mac/documentation/Cocoa/Conceptual/ProgrammingWithObjectiveC/WorkingwithBlocks/WorkingwithBlocks.html#//apple_ref/doc/uid/TP40011210-CH8-SW2)
