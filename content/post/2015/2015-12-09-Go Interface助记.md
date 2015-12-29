---
date: 2015-12-09T12:57:11+08:00
title: "Go Interface助记"
categories:
- 技术文章
tags: [go]

---
Go语言的interface(接口)灵活性很大，也就是Go语言发明者说的Duck类型，理论上简单直白，但是对于我这个半路学习编程的人来说还是犯了一个错误，而且还是多次，这引起了我的重视，决定记录下来。虽然这显得很Low，但没办法，实事求是比较重要。

先前，理解interface是把它当作一个指针，初始化为nil。按照C语言的写法，将一个NULL赋值给一个指针，该指针也是NULL，先看看C语言的代码片段:
```c
typedef struct yellowDuck_s {
  char name[32];
  } yellowDuck;
}

int main(int argc, char *argv[])
{
  void *duck = NULL;

  if (duck == NULL) {
    printf("%s\n", "no duck");  // 输出为no duck
  }

  yellowDuck *yd = NULL;
  
  duck = yd;
  if (duck == NULL) {
    printf("%s\n", "no duck");  // 输出为no duck
  }
  return 0;
}

```
指针指向的地址为NULL，`ptr == NULL` 这样的判断结果都为true。

Go语言这么做会出现问题，`interface == nil`的结果为false，在此处犯了几次错误，引起了一些bug，做了些测试仍然绕不过这个弯，没有理论背景，不知道如何理解。
```go
type duck interface {
  Say()
}

type yellowDuck struct {
  name string
}

type duckHose struct {
  d duck
}

func (d *yellowDuck) Say() {
  fmt.Println(d.name, "GaGa......")
}

func main() {
  var d duck
  
  fmt.Printf("%T\n", d)          // 输出为<nil>
  if d == nil {
    fmt.Println("no duck")       // 输出为: no duck
  }
  
  var d1 *yellowDuck
  var d2 duck
  d2 = d1
  fmt.Printf("%T %T\n", d2, d1)  // 输出为*main.yellowDuck *main.yellowDuck
  if d2 == nil {
    fmt.Println("no duck here")  // 此处不输出，因为d2 == nil条件判断为false
  }
  
  d = &yellowDuck{name: "xiaohuangya"}
  d.Say()
  
  h := &duckHose{d: d1}
  if h.d == nil {
    fmt.Println("empty")         // 此处不输出，同样h.d == nil的结果为false
  }
}
```
在intereface判断是否为nil的情况下，出了几次错误，将d1赋值给d2时`d2 = d1`，`d2 == nil`的结果**false**，但`d1 == nil`的结果为**true**，所以不能拿C语言的指针来看待interface，虽然行为上两者很相似，但两者之间处理的差别很大，在赋值时`d2 = d1`，d2中保存的应该是d1的指针或者引用。所以d2不为nil。

但是，有需要判断**d2**或者**h.d**所指向的值是否为nil，此时需要加**类型断言**才能得到正确的结果
```go
...

if d2.(*yellowDuck) == nil {
  fmt.Println("no duck here")    // 输出为: no duck here
}

...

if h.d.(*yellowDuck) == nil {
  fmt.Println("empty")           // 输出为: empty
}
```

助记:    

1. interface初始化为nil

2. 赋值时，interface中保存的是右值的地址

3. 要判断Interface所指向的值是否为nil，要加类型断言

注: 助记是没有办法的办法，先这么理解。仍然是C语言的思维，肯定有误差，以后理论基础丰富了再来纠错。
