---
layout: post
title: 高效对一个数组去重并输出
date: 2015-10-29 10:08:00
categories: JavaScript
---

js代码如下：

{% highlight ruby %}
var a = [1,2,3,2,1];
var b = [];
var map = {};
for(var i = 0;i<a.length;i++){
	if(!map[a[i]]){
    	map[a[i]]=true;
        b.push(a[i]);
    }
}
console.log(b);
{% endhighlight %}

a即是需要去重的数组，b是用于存储去重后的数组，map对象用于判断a中的元素是否已存在。只需要遍历一次a数组，每次去判断map对象中的a[i]属性是否存在，如果不存在，就将其value设置为true，并放到b数组中；如果存在，表明a数组中已存在此元素，不做处理。如此一来，遍历完一次就将所有重复的元素去除了，但此方法只适用于数组里是非引用类型的元素，因为需要通过map对象的属性名是否相同去判断是否存在某个元素，而引用类型的元素会转换为字符串而无法比较。