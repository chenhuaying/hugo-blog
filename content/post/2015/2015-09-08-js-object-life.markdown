---
layout: post
title: "JS Object Life"
date: 2015-09-08T18:30:12+08:00
categories:
- 技术文章
tags: [Javascript]

---
### Prototype
JavaScript是基于原型的面向对象语言，基本上每个Object（对象）都有一个Prototype（原型），在JS中原型也是一种对象，对象中各个属性的都是来源于它，在对象中引用一个属性时，会向上递归的查找原型。
Object.prototype是大多数对象的原型

1. `Object.create(prototype)`以一个原型为基础来创建一个对象
2. `Object.getPrototypeOf(object)`获取对象的原型

```javascript
function RTextCell(text) {
  TextCell.call(this, text);
}
RTextCell.prototype = Object.create(TextCell.prototype);
```
* 构造函数通常以大写字母开头。

上面是继承机制的一个实例，RTextCell的构造函数调用其父对象TextCell中的构造函数，这样就继承了TextCell的所有属性。

RTextCell的prototype继承TextCelll的prototype，从RTextCell产生的实例就会可以调用TextCell原型中包含的属性。

无原型的构造函数`Object.create(prototype)`

### Interface
```javascript
Object.defineProperty(TextCell.prototype, "heightProp", {
    get: function() { return this.text.length; },
    set: function(value) { this.text.length = value; }
  });
var cell = new TextCell("no\nway");
console.log(cell.heightProp);
// → 2
cell.heightProp = 100;
console.log(cell.heightProp);
// → 100, 如果没有set，此处输出为2，执行不会报错，简单的忽略该操作
```
```javascript
Object.defineProperty(Object.prototype, "hiddenNonsense",
    {enumerable: false, value: "hi"});
for (var name in map)
  console.log(name);
// → pizza
// → touched tree
// 此处不会出现hiddenNonsense，设置enumerable属性为false
console.log(map.hiddenNonsense);
// → hi, hiddenNonsense可以访问，只是不再for/in loop中显示
console.log("hiddenNonsense" in map)
// →true, 在in操作中仍然可见
```
三个参数
Object.defineProperty(**ObjectName**.prototype, **property name**, **options**)
options是一个字典型的参数，其选项有：
1. enumerable, 是否可以在 for/in loop中可见，但在__in__操作中仍可见
2. value, 该property的值
3. setl/get, 读写属性的函数

```javascript
var pile = {
  elements: ["eggshell", "orange peel", "worm"],
  get height() {
    return this.elements.length;
  },
  set height(value) {
    console.log("Ignoring attempt to set height to", value);
  }
};

console.log(pile.height);
// → 3
pile.height = 100;
// → Ignoring attempt to set height to 100
```
* set/get 在这里相当于是**function**这样的标识符, 后面没有冒号，与*Object.defineProperty*中的有区别，后者是*map*中的一个属性，所以需要加":"
