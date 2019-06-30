---
layout: post
title: "JS设计模式--发布/订阅模式"
subtitle: "The mode of publish-subscribe"
date: 2017-11-10 21:00:00
author: "Vinter"
catalog: true
tags:
    - 发布/订阅
---

> 人生缺少目标就好比行尸走肉，目标就是动力。

## 一、前言
JS语言的执行环境是“单线程”，所以任务是一个一个进行执行，如果任务多就需要排队。如果任务多，浏览器加载不过来就会出现无响应状态（假死），因此JS语言将执行模式分为两种：异步和同步。同步很简单，执行一个程序后，才能执行下一个任务。但是异步就相对比较复杂些，一旦有异步程序，会先将同步任务执行完后才去执行异步任务，这只是大概讲了一下他的意思，真正的原理请移步[JS的异步分析](https://www.jianshu.com/p/00e2fdea8d82)。

## 二、发布/订阅模式
发布/订阅模式又称观察者模式，通俗的意思：例如各位买房客户留下信息给某个房地产销售中心，说好如果有房或者楼房开盘前通知我，这样销售中心人员有消息就会将会通知给各位客户，不用客户主动联系楼房销售中心，其中客户充当订阅者，而销售中心就是发布者，这种模式就是发布/订阅者模式。

**应用**
```
document.getElementById("myBtn").addEventListenter("click"，fn1，false);

document.getElementById("myBtn").addEventListenter("click"，fn2，false);

```
相信各位大前端都了解这段代码，事件绑定到这个按钮，所以事件fn1和fn2是订阅者，这个按钮是发布者。
请看下文原理分析

## 三、简单Demo
```
function Public() {
	//  存放订阅者
	this.subscribers = [];

	//  添加订阅者
	this.addSubscriber = (subscriber) => {

		//  保证一个订阅者只能订阅一次
		//  数组some方法，如果存在相同则返回false,不再继续执行,否则返回true
		let isExist = this.subscribers.some(item => {
		  return item === subscriber
		})
		if(!isExist){
		  this.subscribers.push(subscriber)
		}

	}

	this.deliver = data => {
		for(let item of this.subscribers) {
			//	执行
			item(data)
		}
	}
}

//  订阅者
let a = data => {console.log('a===' + data)}
let b = data => {console.log('b===' + data)}

let publisher = new Public()

//  添加订阅者
publisher.addSubscriber(a)
publisher.addSubscriber(b)

//  发布消息
publisher.deliver('你好')

```
这是简单的代码，还没涉及到“click”这一类型事件

## 四、完整Demo
```
function Public() {
	this.handlers = {};
}
Public.prototype = {
    // 订阅事件
    on(eventType, handler) {
        if(!(this.handlers.hasOwnProperty(eventType))) {
           this.handlers[eventType] = [];
        }
        this.handlers[eventType].push(handler);
        return this;
    },
     // 触发事件(发布事件)
    emit(eventType) {
       let handlerArgs = Array.prototype.slice.call(arguments,1);
       for(let i = 0; i < this.handlers[eventType].length; i++) {
         this.handlers[eventType][i].apply(this,handlerArgs);
       }
       return this;
    },
    // 删除订阅事件
    off(eventType, handler) {
        let currentEvent = this.handlers[eventType];
        let len = 0;
        if (currentEvent) {
            len = currentEvent.length;
            for (let i = len - 1; i >= 0; i--){
                if (currentEvent[i] === handler){
                    currentEvent.splice(i, 1);
                }
            }
        }
        return this;
    }
};
 
let Publisher = new Public();

//订阅事件a
Publisher.on('a', function(data){
    console.log(1 + data);
});
Publisher.on('a', function(data){
    console.log(2 + data);
});
 
//触发事件a

Publisher.emit('a', '我是第1次调用的参数').emit('a', '我是第2次调用的参数');
//  Publisher.emit('a', '我是第2次调用的参数');

//  1我是第1次调用的参数
//  2我是第1次调用的参数
//  1我是第2次调用的参数
//  2我是第2次调用的参数
```
```
document.getElementById("myBtn").addEventListenter("click"，fn2，false);
```
现在明白DOM监听的原理了吧