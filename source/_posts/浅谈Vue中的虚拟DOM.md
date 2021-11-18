---
title: 浅谈Vue中的虚拟DOM
tags:
  - 知识点备忘
originContent: ''
categories:
  - Web开发
toc: false
date: 2021-10-21 15:24:36
---

## 前言
Vue2.0引入了虚拟DOM，比Vue1.0的初始渲染速度提升了2～4倍，并大大降低了内存消耗。目前主流的前端框架Vue、React核心技术也都使用了虚拟DOM，你是否好奇为什么要提出虚拟DOM，虚拟DOM是什么，它有什么优势？这一切的问题，都将在本篇揭晓。

## 为什么要提出虚拟DOM
在Web早期，页面的交互比较简单，没有复杂的状态需要管理，也不太需要频繁的操作DOM，随着时代的发展，页面上的功能越来越多，我们需要实现的需求也越来越复杂，DOM的操作也越来越频繁。通过js操作DOM的代价很高，因为会引起页面的重排重绘，增加浏览器的性能开销，降低页面渲染速度，既然操作dom的代价很高那么有没有那种方式可以减少对dom的操作？这就是为什么提出虚拟dom一个很重要的原因。

## 模版转换成视图的过程
在正式介绍Virtual DOM之前，我们有必要先了解下模版转换成视图的整个过程(如下图)：
- Vue.js通过编译将模版转换成渲染函数(render)，执行渲染函数就可以得到一个虚拟DOM
- 在对模型进行操作的时候，会触发对应的Dep中的Watcher对象。Watcher对象会调用对应的update来修改视图。这个过程主要是将新旧虚拟DOM进行差异对比，然后根据结果进行对比。
简单点讲，在Vue的实现上，Vue讲模版编译成虚拟DOM渲染函数。结合Vue自带的响应系统，在状态改变时，Vue能够智能地计算出重新渲染组件的最小代价并应用到DOM操作上。
![image.png](https://blogimage.houjiyi.com/Fl12e31OgbTL-xnjXd_Yyr0ofZmW)

我们先对上图几个概念嵌入解释：

- 渲染函数：渲染函数是用来生成虚拟DOM的。Vue推荐使用模版来构建我们的应用界面，在实现中Vue布局模版编译成渲染函数，当然我们也可以不写模版，直接写渲染函数，这样子更接近编译后的模版。
- vnode虚拟节点：它可以代表一个真实的DOM节点通过createElement方法能将vnode渲染成DOM节点，简单地说，虚拟节点可以理解成节点描述对象，它描述了应该怎样去创建真实的DOM节点。
- patch(也称为patching算法)：虚拟DOM最核心的部分，它可以将vnode渲染成真实的DOM，这个过程是对比新旧虚拟节点之间有哪些不同，然后根据对比结果找出需要更新的的节点进行更新。这点我们从单词含义就可以看出， patch本身就有补丁、修补的意思，其实际作用是在现有DOM上进行修改来实现更新视图的目的。Vue的Virtual DOM Patching算法是基于Snabbdom的实现，并在些基础上作了很多的调整和改进。

## Virtual DOM 是什么？
Virtual DOM 其实就是一棵以 JavaScript 对象( VNode 节点)作为基础的树，用对象属性来描述节点，实际上它只是一层对真实 DOM 的抽象。最终可以通过一系列操作使这棵树映射到真实环境上。

  简单来说，可以把Virtual DOM 理解为一个简单的JS对象，并且最少包含标签名( tag)、属性(attrs)和子元素对象( children)三个属性。不同的框架对这三个属性的命名会有点差别。

对于虚拟DOM，咱们来看一个简单的实例，就是下图所示的这个，详细的阐述了模板 → 渲染函数 → 虚拟DOM树 → 真实DOM的一个过程

![image.png](https://blogimage.houjiyi.com/FkAtIjsxjASDIO_lxH_aD_ZABlLX)

## 为什么虚拟DOM可以提高渲染速度

传统方式用js操作DOM会有很多额外的DOM操作，例如，一个ul标签下有很多个li标签，其中只有一个li有变化，这种情况下如果使用新的ul去替代旧的ul，其实除了那个发生变化的li节点之外，其他节点都不需要重新渲染。由于DOM操作比较慢，所以这些DOM操作在性能上会有一定的浪费，避免这些不必要的DOM操作会提升很大一部分性能(减少重排重绘从而节省浏览器的性能开销)。
  为了避免不必要的DOM操作，虚拟DOM在虚拟节点映射到视图的过程中，将虚拟节点与上一次渲染视图所使用的虚拟节点(oldVnode)做对比，找出真正需要更新的节点来进行DOM操作，从而避免操作其他无任何改动的DOM。
其实虚拟DOM在Vue.js中主要做了两件事：
- 提供与真实DOM节点所对应的虚拟节点vnode
- 将虚拟节点vnode和旧虚拟节点oldVnode进行比对，然后更新视图
对两个虚拟节点进行对比是虚拟DOM中最核心的算法即patch，patch算法的核心是diff算法，它可以判断出哪些节点发生了变化，从而只对发生了变化的节点进行更新操作。

## diff算法
![image.png](https://blogimage.houjiyi.com/FhrRWops52b2yTl2OoF52MLf60r3)

Vue的diff算法是基于snabbdom改造过来的，仅在同级的vnode间做diff，递归地进行同级vnode的diff，最终实现整个DOM树的更新。因为跨层级的操作是非常少的，忽略不计，这样时间复杂度就从O(n3)变成O(n)。

diff 算法包括几个步骤：

- 用 JavaScript 对象结构表示 DOM 树的结构；然后用这个树构建一个真正的 DOM 树，插到文档当中
- 当状态变更的时候，重新构造一棵新的对象树。然后用新的树和旧的树进行比较，记录两棵树差异
- 把所记录的差异应用到所构建的真正的DOM树上，视图就更新了

![image.png](https://blogimage.houjiyi.com/Fra1xRvFBjNvLWoOV5ou2TlZMRfA)

## diff算法的实现过程
diff 算法本身非常复杂，实现难度很大。本文去繁就简，粗略介绍以下两个核心函数实现流程：
patch(container,vnode) :初次渲染的时候，将VDOM渲染成真正的DOM然后插入到容器里面。
patch(vnode,newVnode):再次渲染的时候，将新的vnode和旧的vnode相对比，然后之间差异应用到所构建的真正的DOM树上。

## patch(container,vnode)
通过这个函数可以让VNode渲染成真正的DOM，我们通过以下模拟代码，可以了解大致过程：
```javascript
function createElement(vnode) {    
  var tag = vnode.tag  
  var attrs = vnode.attrs || {}    
  var children = vnode.children || []    
  if (!tag) {       
   return null  
  }    
  // 创建真实的 DOM 元素    
  var elem = document.createElement(tag)   
  // 属性    
  var attrName    
  for (attrName in attrs) {    
    if (attrs.hasOwnProperty(attrName)) { 
           // 给 elem 添加属性
           elem.setAttribute(attrName, attrs[attrName])
        }
    }
    // 子元素
    children.forEach(function (childVnode) {
        // 给 elem 添加子元素，如果还有子节点，则递归的生成子节点。
        elem.appendChild(createElement(childVnode))  // 递归
    })    // 返回真实的 DOM 元素   
 return elem
}
```

## patch(vnode,newVnode)
这里我们只考虑vnode与newVnode如何对比的情况：
```javascript
function updateChildren(vnode, newVnode) {
    var children = vnode.children || []
    var newChildren = newVnode.children || []
    // 遍历现有的children
    children.forEach(function (childVnode, index) {
        var newChildVnode = newChildren[index]
        // 两者tag一样
        if (childVnode.tag === newChildVnode.tag) {
            // 深层次对比，递归
            updateChildren(childVnode, newChildVnode)
        } else { 
           // 两者tag不一样
           replaceNode(childVnode, newChildVnode) 
       }
    }
)}
```

## Virtual DOM的优势
- 具备跨平台的优势
由于 Virtual DOM 是以 JavaScript 对象为基础而不依赖真实平台环境，所以使它具有了跨平台的能力，比如说浏览器平台、Weex、Node 等。

- 操作 DOM 慢，js运行效率高。我们可以将DOM对比操作放在JS层，提高效率。
因为DOM操作的执行速度远不如Javascript的运算速度快，因此，把大量的DOM操作搬运到Javascript中，运用patching算法来计算出真正需要更新的节点，最大限度地减少DOM操作，从而显著提高性能。

Virtual DOM 本质上就是在 JS 和 DOM 之间做了一个缓存。可以类比 CPU 和硬盘，既然硬盘这么慢，我们就在它们之间加个缓存：既然 DOM 这么慢，我们就在它们 JS 和 DOM 之间加个缓存。CPU（JS）只操作内存（Virtual DOM），最后的时候再把变更写入硬盘（DOM）

- 提升渲染性能
Virtual DOM的优势不在于单次的操作，而是在大量、频繁的数据更新下，能够对视图进行合理、高效的更新。

> 总结：Vue.js通过编译将模版转换成渲染函数(render)，执行渲染函数就可以得到一个虚拟节点树(虚拟DOM)，虚拟节点树(虚拟DOM)提供虚拟节点vnode和对新旧两个vnode进行比对并根据比对结果进行DOM操作来更新视图，达到减少对DOM的目的，从而减少浏览器的开销，提高渲染速度，改善用户体验。

参考文章：
https://github.com/ljianshu/Blog/issues/69
https://book.douban.com/subject/32581281/

原文地址：https://www.jianshu.com/p/4dbb3712ced7