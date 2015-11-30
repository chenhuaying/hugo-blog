---
layout: post
title: "Golang Struct Reset"
date: 2015-09-24T12:20:03+08:00
categories:
- 技术文章
tags: [Go]
---
直接零赋值
```
type mystrut struct {
	name string
	data interface{}
}

ins := &mystruct{
	name: "Golang newbie"
	data: "string data"
}

ins = &mystruct{}
```

或者用反射(**reflect**)清零

```
func clear(v interface{}) {
	p := reflect.ValueOf(v).Elem()
	p.Set(reflect.Zero(p.Type()))
}
```
如果传入的参数不是指针会直接pannic
