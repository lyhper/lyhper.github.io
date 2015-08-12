---
layout: post
title: "JavaScript实现继承"
date: 2015-08-08 22:36
categories: JavaScript
header-img: img/JavaScript-inheritance.jpg
---

JavaScript主要通过以下6种方式来实现对象的继承，包括`原型链`，`构造函数`，`组合继承`，`原型式继承`，`寄生式继承`，`寄生组合式继承`

##原型链

此方法主要是通过将父类的实例赋值给子类的原型，从而将父类的属性与方法存于子类的原型中，实现继承。代码如下：

{% highlight ruby %}
function Food(){
  this.isEatable=true;
}
Food.prototype.getIsEatable=function(){
  alert(this.isEatable);
}
function Fruit(){
  this.hasWater=true;
}

//实现继承
Fruit.prototype=new Food();

Fruit.prototype.getHasWater=function(){
  alert(this.hasWater);
}
var fruit1=new Fruit();
fruit1.getIsEatable();//true
console.log(fruit1 instanceof Food);//true
{% endhighlight %}

当要查找某个属性或方法时，会沿着原型链向上查找，直到找到为止。即首先查找实例，然后查找子类的原型，最后查找父类的原型。

##构造函数

这种方法的主要思想是在子类的作用域中调用父类的构造函数实现继承。代码如下：

{% highlight ruby %}
function Food(){
  this.isEatable=true;
}
function Fruit(){
  this.hasWater=true;
  //实现继承
  Food.call(this);
}
var fruit1=new Fruit();
console.log(fruit1.isEatable);//true
{% endhighlight %}

构造函数的方法实现继承也会存在一个问题，那就是方法的复用。因此，实际应用中很少会单独使用构造函数的方法。








