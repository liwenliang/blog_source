---
title: JS中call()和apply()
tags:
  - 知识点备忘
originContent: ''
categories:
  - Web开发
toc: false
date: 2020-07-23 15:24:40
---

## 每个函数都包含两个非继承而来的方法：call()和apply()；

在JavaScript中，call和apply作用是一样的，都是为了改变某个函数运行时的上下文（context）而存在的，换句话说，就是为了改变函数体内部this的指向。
```javascript
function fruits(){}
        
fruits.prototype = {
    color: "red",
    say: function(){
        console.log("My color is " + this.color);
    }
};

var apple = new fruits;
apple.say();                //My color is red
```

当想另外一个对象想使用fruits中的say方法时不用重新写，使用call和apply可以实现“劫持”别人的方法。

```javascript
function fruits(){}
            
fruits.prototype = {
    color: "red",
    say: function(){
        console.log("My color is " + this.color);
    }
};

var another = {
    color: "yellow"
};

var apple = new fruits;
apple.say();                //My color is red
apple.say.call(another);    //My color is yellow
apple.say.apply(another);   //My color is yellow
```

## 区别：参数书写方式不同
call(thisObj, arg1, arg2, arg3, arg4);
apply(thisObj, [args]);
thisObj：call和apply第一个参数是一样的，该参数将替代Function类里面的this对象。
arg1,arg2....：是一个个的参数，
args：一个数组或类数组，是一个参数列表。

## 用法
改变函数作用域

```javascript
var name = "小白";
var obj = {
    name: "小红"
};

function sayName() {
    return this.name;
}
console.log(sayName.call(this));   //小白
console.log(sayName.call(obj));    //小红
```

## 实现继承

```javascript
//实现js继承
//父类
function Person(name, height) {
    this.sayInfo = function() {
        return "姓名：" + name + ", 身高：" + height + ", 体重：" + this.weight;
    }
}
//子类
function Chinese(name, height, weight) {
    Person.call(this, name, height);
    this.weight = weight;
    
    this.nation = function() {
        console.log("我是中国人");
    }
}
//子类
function America(name, height, weight) {
    Person.apply(this, [name, height]);
    this.weight = weight;
}

let chiness = new Chinese("成龙", "178cm", "60kg");
console.log(chiness.sayInfo());    //姓名：成龙, 身高：178cm, 体重：60kg
let america = new America("jack", "180cm", "55kg");
console.log(america.sayInfo());    //姓名：jack, 身高：180cm, 体重：55kg
```

作者：书虫和泰迪熊
链接：https://www.jianshu.com/p/aa2eeecd8b4f
来源：简书
著作权归作者所有。商业转载请联系作者获得授权，非商业转载请注明出处。