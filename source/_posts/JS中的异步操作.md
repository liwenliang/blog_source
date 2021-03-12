---
title: JS中的异步操作
tags:
  - 知识点备忘
originContent: ''
categories:
  - Web开发
toc: false
date: 2020-07-24 09:56:52
---

JS中异步编程的方法有：

- 回调函数
- 事件监听
- 发布/订阅
- promise
- generator（ES6）
- async/await(ES7)


## 回调函数
回调是异步编程中最基础的方法。举例一个简单的回调：在f1执行完之后再执行f2

```javascript
var func1=function(callback){
    console.log(1);
    (callback && typeof(callback)==='function') && callback();
}
func1(func2);
var func2=function(){
    console.log(2);
}
```

异步回调中最常见的形式可能就是Ajax了：
```javascript
$.ajax({
    url:"/getmsg",
    type: 'GET',
    dataType: 'json',
    success: function(ret) {
        if (ret && ret.status) {
            //
        }
    },
    error: function(xhr) {
        //
    }
})
```

## 事件监听
通过事件机制，实现代码的解耦。js处理DOM交互就是采用的事件机制，我们这儿只是实现一些自定义的事件而已。JS中已经很好的支持了自定义事件，如：
```javascript
//新建一个事件
var event=new Event('Popup::Show');
//dispatch the event
elem1.dispatchEvent(event)

//listen for this event
elem2.addEventListener('Popup::Show',function(msg){},false)
```

## 发布-订阅模式
在系统中存在一个"信号中心"，当某个任务执行完成后向信号中心"发布"（publish）一个信号，其他任务可以向信号中心"订阅"（subscribe）这个信号，从而知道什么时候自己可以开始执行。简单实现如下：
```JavaScript
//发布-订阅
//有个消息池，存放所有消息
let pubsub = {};
(function(myObj) {
    topics = {}
    subId = -1;
    //发布者接受参数(消息名称，参数)
    myObj.publish = function(topic, msg) {
            //如果发布的该消息没有订阅者，直接返回
            if (!topics[topic]) {
                return
            }
            //对该消息的所有订阅者，遍历去执行各自的回调函数
            let subs = topics[topic]
            subs.forEach(function(sub) {
                sub.func(topic, msg)
            })
        }
    //订阅者接受参数：（消息名称，回调函数）
    myObj.subscribe = function(topic, func) {
        //如果订阅的该事件还未定义，初始化
        if (!topics[topic]) {
            topics[topic] = []
        }
        //使用不同的token来作为订阅者的索引
        let token = (++subId).toString()
        topics[topic].push({
                token: token,
                func: func
            })
        return token
    }
    myObj.unsubscribe = function(token) {
        //对消息列表遍历查找该token是哪个消息中的哪个订阅者
        for (let t in topics) {
            //如果某个消息没有订阅者，直接返回
            if (!topics[t]) {
                return }
            topics[t].forEach(function(sub,index) {
                if (sub.token === token) {
                    //找到了，从订阅者的数组中去掉该订阅者
                    topics[t].splice(index, 1)
                }
            })
        }
    }
})(pubsub)

let sub1 = pubsub.subscribe('Msg::Name', function(topic, msg) {
    console.log("event is :" + topic + "; data is :" + msg)
});
let sub2 = pubsub.subscribe('Msg::Name', function(topic, msg) {
    console.log("this is another subscriber, data is :" + msg)
});
pubsub.publish('Msg::Name', '123')

pubsub.unsubscribe(sub2)
pubsub.publish('Msg::Name', '456')
```

其中存储消息的结构用json可以表示为：
```javascript
topics = {
    topic1: [{ token: 1, func: callback1 }, { token: 2, func: callback2 }],
    topic2: [{ token: 3, func: callback3 }, { token: 4, func: callback4 }],
    topic3: []
}

```

消息池的结构是发布订阅模式与事件监听模式的最大区别。当然，每个消息也可以看做是一个个的事件，topics对象就相当于一个事件处理中心，每个事件都有各自的订阅者。所以事件监听其实就是发布订阅模式的一个简化版本。而发布订阅模式的优点就是我们可以查看消息中心的信息，了解有多少信号，每个信号有多少订阅者。

## 再说一说观察者模式
很多情况下，我们都将观察者模式和发布-订阅模式混为一谈，因为都可用来进行异步通信，实现代码的解耦，而不再细究其不同，但是内部实现还是有很多不同的。

1. 整体模型的不同：发布订阅模式是靠信息池作为发布者和订阅者的中转站的，订阅者订阅的是信息池中的某个信息；而观察者模式是直接将订阅者订阅到发布者内部的，目标对象需要负责维护观察者，也就是观察者模式中订阅者是依赖发布者的。

2. 触发回调的方式不同：发布-订阅模式中，订阅者通过监听特定消息来触发回调；而观察者模式是发布者暴露一个接口(方法)，当目标对象发生变化时调用此接口，以保持自身状态的及时改变。

观察者模式很好的应用是MVC架构，当数据模型更新时，视图也发生变化。从数据模型中将视图解耦出来，从而减少了依赖。但是当观察者数量上升时，性能会有显著下降。我们同样可以自己实现：
```javascript
//观察者模式
var Subject=function(){
    this.observers=[];
}
Subject.prototype={
    subscribe:function(observer){
        this.observers.push(observer);
    },
    unsubscribe:function(observer){
        var index=this.observers.indexOf(observer);
        if (index>-1) {
            this.observers.splice(index,1);
        }
    },
    notify:function(observer,msg){
        var index=this.observers.indexOf(observer);
        if (index>-1) {
            this.observers[index].notify(msg)
        }
    },
    notifyAll:function(msg){
        this.observers.forEach(function(observe,msg){
            observe.notify(msg)
        })
    }
}
var Observer=function(){
    return {
        notify:function(msg){
            console.log("received: "+msg);
        }
    }
}
var subject=new Subject();
var observer0=new Observer();
var observer1=new Observer();
var observer2=new Observer();
var observer3=new Observer();
subject.subscribe(observer0);
subject.subscribe(observer1);
subject.subscribe(observer2);
subject.subscribe(observer3);
subject.notifyAll('all notified');
subject.notify(observer2,'asda');
```

## promise

为解决回调函数噩梦而提出的写法，将回调函数的横向加载变成纵向加载。

对象状态不受外界影响。三种状态：pending,resolved,rejected。只有异步操作的结果才能改变状态
状态一旦改变，就不会再变。
用Promise对象实现Ajax操作的例子

```javascript
var getJSON=function(url){
    var promise=new Promise(function(resolve,reject){
        var client=new XMLHttpRequest();
        client.open("GET",url);
        client.onreadystatechange=handler;
        client.responseType="json";
        client.setRequestHeader("Accept","application/json");
        client.send();
        function handler(){
            if(this.readyState!=4){
                return;
            }
            if(this.status==200){
                resolve(this.response);
            }else{
                reject(new Error(this.statusText));
            }
        }
    });
    return promise;
}

getJSON('/posts.json').then(function(json){
    console.log('Contents: '+json);
},function(error){
    console.error(error)
})
```

再举一个需要多层回调的例子：假设每个步骤都是异步，并且依赖上一个步骤的结果，使用setTimeout来模拟异步操作。
```javascript
//输入n，表示该函数执行时间，结果为n+200,并且用于下一步的输入
function takeLongTime(n){
    return new Promise(resolve=>{
        setTimeout(()=>resolve(n+200),n)
    })
}

function step1(n){
    console.log(`step1 with ${n}`);
    return takeLongTime(n);
}

function step2(n){
    console.log(`step2 with ${n}`);
    return takeLongTime(n);
}

function step3(n){
    console.log(`step3 with ${n}`);
    return takeLongTime(n);
}

```

如果使用Promise的方式将其3个步骤处理为链式操作，每一步都返回一个promise对象，将输出的结果作为下一步新的输入：

```javascript
function dolt(){
    console.time('dolt');
    const time1=300;
    step1(time1)
    .then(time2=>step2(time2))
    .then(time3=>step3(time3))
    .then(result=>{
        console.log(`result is ${result}`);
        console.timeEnd('dolt')
    });
}
dolt();
//输出结果为
step1 with 300
step2 with 500
step3 with 700
result is 900
dolt: 1516.713ms
```

实际耗时跟我们计算的延迟时间300+500+700=1500ms差不多。但是对于长的链式操作来说，看起来是一堆then方法的堆砌，代码冗余，语义也不清楚，而且还是靠着箭头函数才使得代码略微简短一些。Promise还有一个痛点，就是传递参数太麻烦，尤其是需要传递多参数的情况下。

## Generator函数
generator是一个封装的异步任务，在需要暂停的地方，使用yield语句注明。如

```javascript
function* gen(x){
    let y=yield x+2;
    return y;
}
let g=gen(1);
g.next();
//返回 {value: 3, done: false}
g.next();
//返回 {value: undefined, done: true}
```

调用generator函数返回的是内部的指针对象，调用next方法就会移动内部指针。Generator函数之所以能被用来处理异步操作，因为它可以暂停执行和恢复执行、函数体内外的数据交换和错误处理机制。

针对前面多任务的例子，使用generator实现：
```javascript
function* dolt(){
    console.time('dolt');
    const time1=300;
    const time2=yield step1(time1);
    const time3=yield step2(time2);
    const result=yield step3(time3);
    console.log(`result is ${result}`);
    console.timeEnd('dolt');
}
```

但是 Generator 函数的执行必须靠执行器
```javascript
function spawn(genF) {
  return new Promise(function(resolve, reject) {
    var gen = genF();
    function step(nextF) {
      try {
        var next = nextF();
      } catch(e) {
        return reject(e); 
      }
      if(next.done) {
        return resolve(next.value);
      } 
      Promise.resolve(next.value).then(function(v) {
        step(function() { return gen.next(v); });      
      }, function(e) {
        step(function() { return gen.throw(e); });
      });
    }
    step(function() { return gen.next(undefined); });
  });
}
spawn(dolt);

```

## async/await
async函数基于Generator又做了几点改进：

- 内置执行器，将Generator函数和自动执行器进一步包装。
- 语义更清楚，async表示函数中有异步操作，await表示等待着紧跟在后边的表达式的结果。
- 适用性更广泛，await后面可以跟promise对象和原始类型的值(Generator中不支持)


很多人都认为这是异步编程的终极解决方案，由此评价就可知道该方法有多优秀了。它基于Promise使用async/await来优化then链的调用,其实也是Generator函数的语法糖。 async 会将其后的函数（函数表达式或 Lambda）的返回值封装成一个 Promise 对象，而 await 会等待这个 Promise 完成，并将其 resolve 的结果返回出来。

await得到的就是返回值，其内部已经执行promise中resolve方法，然后将结果返回。使用async/await的方式重写前面的回调任务：

```javascript
async function dolt(){
    console.time('dolt');
    const time1=300;
    const time2=await step1(time1);
    const time3=await step2(time2);
    const result=await step3(time3);
    console.log(`result is ${result}`);
    console.timeEnd('dolt');
}

dolt();
```

功能还很新，属于ES7的语法，但使用Babel插件可以很好的转义。另外await只能用在async函数中，否则会报错。

作者：RichardBillion
链接：https://www.jianshu.com/p/6f91e7696b91
来源：简书
著作权归作者所有。商业转载请联系作者获得授权，非商业转载请注明出处。