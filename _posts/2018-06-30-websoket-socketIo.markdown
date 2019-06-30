---
layout: post
title: "Websocket你真的了解么？"
subtitle: "Introduction about websocket and Socket.io"
date: 2018-06-30 22:00:00
author: "Vinter"
catalog: true
tags:
    - Websocket
    - Socket.io
---

> 生于忧患死于安乐，身体才是革命的本钱。

最近有点肠胃消化不良，吃东西容易饱腹，而且还有一些嗳气，甚至有点反胃，然后总觉得有问题，后来才知道是天气太热，总是吹空调，导致湿气有点重了，刮痧后舒服多了，人就是这样，只有在生病或者感觉身体不适的情况下，才发现自己以前是不怎么爱护自己的身体，不管工作或者生活多么的累，压力多大，自己的身体真的需要爱护，可能做IT的我们，睡的比较晚，晚点睡觉倒是没什么，主要是要睡足觉，并且睡好了。这只是一个例子，其实更多的时候是我们缺少一种忧患意识，就是什么事情好了一段时间之后，你就忘记了痛苦或者曾经决定做的事情，名言说的好“生于忧患死于安乐”，这话真的没毛病，一定要谨记，一个好的身体才是革命的本钱，而且最重要的是坚持爱护自己，如果连自己都不爱护，保养好，你又如何去呵护身边的人呢。

言归正传，今天的话题是介绍Websocket和socket.io的区别和使用。。。

## 一、Websocket
Websocket是HTML5新增的一种全双工通信协议，客户端和服务端基于TCP握手连接成功后，两者之间就可以建立持久性的连接，实现双向数据传输。

**传统HTTP和Websocket的异同**

**不同点**
1. HTTP是单向数据流，客户端向服务端发送请求，服务端响应并返回数据；Websocket连接后可以实现客户端和服务端双向数据传递。

2. 由于是新的协议，HTTP的url使用"http//"或"https//"开头；Websocket的url使用"ws//"开头。

**相同点**

1. 都需要建立TCP连接

2. 都是属于七层协议中的应用层协议

传统通过HTTP请求模拟双向数据传递的方式是**http+Polling(轮询)和http+Long Polling(长轮询)**。

**轮询(Polling)：**就是客户端定时发送get请求向服务端请求数据，这种方式能满足一定的需求，但是存在一些问题，如果服务端没有新数据，但是客户端get请求到的数据都是旧数据，这样不仅浪费了带宽资源，而且占用CPU内存。

**LongPolling(长轮询)：**就是在Polling上的一些改进，即如果服务端没有新数据返回给客户端，服务端会把当前的这个get请求保持住(hold)，当有新数据时则返回新数据，如果超过一定时间服务端仍没有新数据，则服务端返回超时请求，客户端接收到超时请求，然后在发送get请求，一直循环执行。虽然一定程度解决了带宽资源和CPU内存浪费情况，但是当服务端数据更新很快，这和轮询（Polling）没有本质上的区别，而且http数据包的头部数据量往往很大（通常有400多个字节），但是真正被服务器需要的数据却很少（有时只有10个字节左右），这样的数据包在网络上周期性的传输，难免对网络带宽是一种浪费。在高并发的情况下，这对服务器是一个很大的挑战。综合上面轮询的种种问题，Websocket终于华丽的登上了舞台。
Websocket，这里简单说明一下的使用方式。

```
const ws = new WebSocket("ws//:xxx.xx", [protocol])

ws.onopen = () => {
	ws.send('hello')
	console.log('send')
}

ws.onmessage = (ev) =>{
	console.log(ev.data)
	ws.close()
}

ws.onclose = (ev) =>{
	console.log('close')
}

ws.onerror = (ev) =>{
	console.log('error')
}
```
Websocket实例化后，前端可以使用以上介绍的方法进行对应的操作，这些方法使用比较简单，这里就不多介绍，由于想完全搭建一个Websocket服务端比较麻烦，又耗费时间，接下来主要讲解实际应用中与Websocket相关的库。

## 二、Socket.io
实际应用中，如果需要Websocke进行双向数据通信，[Socket.io](https://github.com/socketio/socket.io)是一个非常好的选择。其实github上面也有通过JS封装好的Websocket库，[ws](https://github.com/websockets/ws)可用于client和基于node搭建的服务端使用，但是用起来相对繁琐，star相对Socket.io较少，所以不推荐使用。

Socket.io不是Websocket，它只是将Websocket和轮询 （Polling）机制以及其它的实时通信方式封装成了通用的接口，并且在服务端实现了这些实时机制的相应代码。也就是说，Websocket仅仅是 Socket.io实现实时通信的一个子集。因此Websocket客户端连接不上Socket.io服务端，当然Socket.io客户端也连接不上Websocket服务端。

## 三、简单聊天实例
servce.js(服务端)

```
'use strict'

let express = require('express')
let path = require('path')
let app = express()
let server = require('http').createServer(app)
let io = require('socket.io')(server)

app.use('/', (req, res, next) => {
	res.status(200).sendFile(path.resolve(__dirname, 'index.html'))
})

io.on('connection', client => {
	console.log(client.id, '=======================')
	client.on('channel', data =>{
		console.log(data)
		io.emit('broadcast', data)
		// client.emit('channel', data)
	})
	client.on('disconnect', () =>{
		console.log('close')
	})
})

server.listen(3000, () => {
	console.log("The service listening on 3000 port")
})
```
index.html(客户端)
```
<!DOCTYPE html>
<html lang="en">
<head>
	<meta charset="UTF-8">
	<title>app</title>
	<script src="https://cdn.bootcss.com/socket.io/2.2.0/socket.io.slim.js"></script>
</head>
<body>
	<input type="text" id="input">
	<button id="btn">send</button>
	<div id="content-wrap"></div>
</body>
	<script>
		window.onload = () => {
			let inputValue = null

			let socket = io('http://localhost:3000')
			socket.on('broadcast', data =>{
				let content = document.createElement('p')
				content.innerHTML = data
				document.querySelector('#content-wrap').appendChild(content)
			})
			
			let inputChangeHandle = (ev) => {
				inputValue = ev.target.value
			}
			let inputDom = document.querySelector("#input")
			inputDom.addEventListener('input', inputChangeHandle, false)

			let sendHandle = () => {				
				socket.emit('channel', inputValue)
			}
			let btnDom = document.querySelector("#btn")
			btnDom.addEventListener('click', sendHandle, false)
			

			window.onunload = () => {
				btnDom.removeEventListener('click', sendHandle, false)
				inputDom.removeEventListener('input', inputChangeHandle, false)
			}
		}
	</script>

</html>
```

新开几个客户端页面，其中一个页面输入value，其他几个页面马上收到value消息，简单的聊天页面就完成了，其中，客户端index.html页面不用服务端返回，自己随便创建个index.html是一样的。