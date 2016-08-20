---
layout: post
title: Node.js异步解决方案之async
categories: Node.js
header-img: img/nodejs-async-bg.jpg
date: 2016-08-20
---
很久没有更新blog，趁今天周末闲下来总结下。

Nodejs中一个很常见的问题就是异步回调函数的多层嵌套（如下图所示），使我们的代码难于维护。

![异步函数嵌套]({{ site.url }}/img/async-nested.png)

之所以会出现这个问题，是因为我们的有些操作是需要同步执行的，比如一次数据库的查找操作依赖于另一次数据库查找，此时，我们只能将第二次数据库查找操作传入第一次查找操作的回调函数里，待第一次操作完成后执行。如此一来，当依赖越来越多的时候，就会出现层层嵌套的代码。而[async](https://github.com/caolan/async)就是解决这个问题的。

## async常用操作

### each——并行地对一个集合中的每个元素执行一个异步操作

{% highlight ruby %}
var async = require('async');
var mysql = require('mysql');

var con = mysql.createConnection({
    ...
});

var sqls = [
    'insertSQL',
    'selectSQL',
    'deleteSQL',
    'updateSQL'
];

async.each(sqls,function(item,callback){
	con.query(item,function(err,res){
		console.log(res);
		callback(err);
	})
},function(err){
	if(err){
		console.log(err);
	}else{
		console.log('success');
	}	
});
{% endhighlight %}

each函数并行地执行一系列异步操作，因此这一系列操作并不一定会按顺序完成。当所有操作均完成或出现某一个操作报错即调用最后一个参数这个函数。

### map——并行地对一个集合中的每个元素执行一个异步操作，并返回结果

{% highlight ruby %}
var async = require('async');
var mysql = require('mysql');

var con = mysql.createConnection({
    ...
});

var sqls = [
    'insertSQL',
    'selectSQL',
    'deleteSQL',
    'updateSQL'
];

async.map(sqls,function(item,callback){
	con.query(item,function(err,res){
		console.log(res);
		// 相比each多传一个res
		callback(err, res);
	})
},function(err, results){
	if(err){
		console.log(err);
	}else{
		console.log(results);
	}	
});
{% endhighlight %}

map类似于each，唯一的不同在于最终会返回一个结果数组

### series——串行地执行一系列任务，并返回结果

{% highlight ruby %}
var async = require('async');

async.series([
	function(callback){
		...
		callback(null, 1);
	},
	function(callback){
		...
		callback(null, 2);
	},
	function(callback){
		...
		callback(null, 3);
	}
],
function(err, results){
	if(err){
		console.log(err);
	}else{
		console.log(results);
	}	
});
{% endhighlight %}

series函数会按数组中注册的函数顺序依次执行，即当第一个异步操作执行完成后才会执行下一个异步操作，这样一来我们便摆脱了多层嵌套的情况。

### eachSeries——串行地对一个集合中的每个元素执行一个异步操作

{% highlight ruby %}
var async = require('async');
var mysql = require('mysql');

var con = mysql.createConnection({
    ...
});

var sqls = [
    'insertSQL',
    'selectSQL',
    'deleteSQL',
    'updateSQL'
];

async.eachSeries(sqls,function(item,callback){
	con.query(item,function(err,res){
		console.log(res);
		// 不关注结果
		callback(err);
	})
},function(err){
	if(err){
		console.log(err);
	}else{
		console.log('success');
	}	
});
{% endhighlight %}

### waterfall——串行地执行一系列异步操作，且每个操作能够接收上一个操作传入的数据

{% highlight ruby %}
var async = require('async');

async.waterfall([
	function(callback){
		...
		callback(null, 1);
	},
	function(arg1, callback){
		...
		callback(null, 2, 3);
	},
	function(arg1, arg2, callback){
		...
		callback(null, 3);
	}
],
function(err, result){
	if(err){
		console.log(err);
	}else{
		// 结果为最后一个操作传入的数据，即3
		console.log(result);
	}	
});
{% endhighlight %}

当所有操作执行完成且并无错误时，最后一个函数将立即执行，并且接收最后一个异步操作传入的数据。或者当执行一系列异步操作时，一旦出错，立马将调用最后一个函数。waterfall函数适用于每一步异步操作依赖于上一步异步操作所获取的数据。