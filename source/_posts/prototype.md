---
title: prototype && __proto__
date: 2016-12-08 14:15:16
tags: [javaScript,Object,prototype]
---


### 导语

> 懒癌晚期，博客搭建好没管都半个多月了，当时想的是，遇到什么难懂的时候再来写写，结果就拖到了现在，其实要深究的话，js基础还是有些没有深入的（比如说 1 && 3的返回值是3），到了对象的原型prototype，刚开始接触真是有点懵，现在也还是有点似懂非懂的，所以还是写一篇博客来备忘吧，也是作为博客生涯的开端。
可能有错误，毕竟也没接触多久，如果发现错误，望指正，可以通过我的email联系到我(gromwild@foxmail.com) 。

<!--more-->

## 1.原型是什么

* 原型prototype是一个对象，prototype的作用是共享

无论什么时候，只要创建了一个新函数，就会根据一组特定的规则创建一个prototype属性，所有的原型对象都会自动获得一个construtor属性，这个属性指向prototype所在函数的指针:

object.prototype.constuctor 指向 object

上面在chrome控制台可以无限展开qwq..

prototype对象是js中所有函数都有的一个属性，主要还是用于构造函数，当年有一个基友对我说过，js是面向函数的编程，到了这里，似乎还是有一点道理的。

* \_\_proto\_\_也是一个对象

\_\_proto\_\_是每一个实例对象中都有的属性，它的指向是对象中结构体constructor的原型, \_\_proto\_\_，\_\_proto\_\_

var obj = new object();
obj.\_\_proto\_\_ = obj.constroctor.prototype;

具体还是上代码吧


```bash

    // 每一个函数都是有prototype对象的
	function Person(){}
		console.log(typeof Person.prototype);    // object
		// 一个包含两个两个属性的对象 constructor 和 __proto__
		console.log(Person.prototype);    

```

## 2.实例创建

```bash

    // 实例属性和方法（个性） 原型中属性和方法（共性）
	function Student(name, age){
        this.name = name;
        this.age = age;
	}

    // 原型上添加的属性和方法简单理解就是直接绑定给实例对象
      // 而对于原型属性和方法可以通过 构造函数.prototype来访问
	Student.prototype.study = function(){
		console.log(this.name+"好好学习");
	}

	Student.prototype.hobby = "lol";
	console.log(Student);   //添加的原型属性和方法并不会在构造函数中体现
	
	var stu1 = new Student("xz",23);
	// 因为原型是函数的属性，是一个对象，实例对象只通过__proto__来访问它的构造函数的原型
	console.log(stu1);
    
    // 对象的实例属性访问和方法调用
    console.log(stu1.name);
    stu1.study();
    
    console.log(stu1.__proto__ == stu1.constructor.prototype);  //true

```
## 3.深入

写了这么久，也就只是简单的说了prototype和\_\_proto\_\_是什么，还是懵，到底他们是啥呢？

这就要说继承了，有一种方式叫原型链继承，上面也说了，可以通过\_\_proto\_\_来找到它的构造函数的原型，而原型链就是链接两个具有继承关系的对象。

```bash
    
    console.log(stu1.__proto__);             // Student
    console.log(stu1.__proto__.__proto__);   // Object
    console.log(stu1.__proto__.__proto__.__proto__);   // null

```

上面就是一个普通的原型链了(Student --> Object --> null)，原型链的顶端居然是 null， 我查了查，这是js的历史遗留问题，具体可以google"the history of "'typeof null'",终于领略到了js的一切皆对象。

## 4.总结

* 原型 prototype 是函数的

* 而 \_\_proto\_\_ 是所有对象都有的，除了null..这个特殊的对象

* 原型的作用是用来继承的，\_\_proto\_\_和prototype的区别就是 \_\_proto\_\_是访问对象的构造函数的原型，而prototype是设置构造函数的原型，用于共享的原型。
