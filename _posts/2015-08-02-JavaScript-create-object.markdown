---
layout: post
title: "JavaScript创建对象"
date: 2015-08-02 20:17
categories: JavaScript
header-img: img/JavaScript-create-object.jpg
---

JavaScript主要通过八种方式创建对象，包括`创建Object的实例`，`工厂模式`，`构造函数模式`，`原型模式`，`构造函数和原型模式相结合`，`动态原型模式`，`寄生构造函数模式`,`稳妥构造函数模式`。

##创建Object的实例

最简单的方式是创建一个Object的实例，然后再为其添加属性和方法，如下所示：
{% highlight ruby %}
var person=new Object();
//添加属性
person.name="xiaoming";
//添加方法
person.sayName=function(){
  alert(this.name);
}
{% endhighlight %}
但使用这种方法有一个缺点，就是会产生大量冗余代码。




