---
layout: post
title: "Something With Golang"
date: 2015-10-21T17:11:27+08:00
categories:
- 技术文章
tags: [Go]
---
### glog

#### vmodule 选项

### 数组vs切片

数组与切片不是一个类型，在以切片为参数的函数时，需要将数组转换为切片, 方法如下：
```go
array := [...]int{1, 2, 3}
func testFunc(a []int) {
  // do something
}

testFunc(array[:])
```
