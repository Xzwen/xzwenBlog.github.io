---
layout:       post
title:        "简单好用的canvas库 —— Fabric.js"
subtitle:     "如何通过Fabric.js打造一个前端高级画板"
date:         2018-11-18 15:30:00
author:       "Vinter"
header-img:   "img/post-bg-js-version.jpg"
header-mask:  0.3
catalog:      true
tags:
    - 前端开发
    - Fabric.js
    - Canvas
---

最近公司需要在项目中加入手写功能，前端能实现这个功能非canvas莫属了，如果自己写不仅费事又费力，主要项目紧，考虑后还是去找找开源的库吧，然后去github， Goole等搜索了一下，发现[Fabric.js](http://fabricjs.com/)非常不错，github上的start已经超过11k了，该库对canvas进行了很好的封装，功能齐全，不仅有各种绘画功能，还有多种事件，支持canvas对象操作，缺点就是官网的文档真的很烂，通过各种其他demo，文档总结了一下Fabric.js，[github地址](https://github.com/fabricjs/fabric.js)，希望对各位前端朋友有所帮助；

## Fabric.js 简介

Fabric.js是可以简化canvas编写的js库，提供canvas缺少的对象模型，包含事件监听、动画、数据序列号和反序列化等高级功能的js库

## Fabric.js 主要功能

* canvas简单创建生成

* 设置画笔样式，自由绘制图形

* 画圆/椭圆/矩形/直角三角形/普通三角形/等边三角形

* 绘制图片、文字、图片剪切等

* 给图形设置渐变色

* 组合图形（图图组合、文图组合等）

* 设置canvas对象动画

* 监听canvas对象事件，简单实现用户交互

* 序列化和反序列化，生成JSON、SVG等数据

## Fabric.js 功能详解

**1. canvas生成**

```
let canvas = new fabric.Canvas("canvas-id")
```

**2. 简单绘制图形**

* 绘制方形： let rect = new fabric.Rect()

* 绘制圆形： let circle = new fabric.Circle()

* 绘制三角形： let triangle = new fabric.Triangle()

* 绘制不规则路径： let path = new fabric.Path()

* 绘制椭圆： let ellipse = new fabric.Ellipse()

* 绘制多边形： let polygon = new fabric.Polygon()

* 绘制直线： let line = new fabric.Line()

* 自由绘画： let freeDrawingBrush = new fabric.FreeDrawingBrush() 

```
//  绘制方形
let rect = new fabric.Rect({
    left: 100,
    top: 100,
    fill: "green",
    width: 50,
    heigth: 50
})

canvas.add(rect)    //  添加图形


//  绘制虚线
let line = new fabric.Line([10, 10, 50, 50], {
fill: 'green',
stroke: 'green',
strokeDashArray:[3,1] 
});
canvas.add(line);
strokeDashArray[a,b]    // 每隔a个像素空b个像素。
```

**3. 绘制图形和文字**

* 绘制文字

```
let text = new fabric.Text('Vinter', {
    fontSize: 20,
    fontWeight: 'bold',
    fontFamily: 'song'
    fill: 'red'
    //  ...
})
canvas.add(text)
```

* 绘制图片

1、背景添加
<br/>

```
fabric.Image.fromURL('../vinter.png', function(img){
    img.scale(0.1)
    //  对图片的一些操作
    canvas.add(img)
})

```
2、创建DOM添加图片

```
let oimg = document.createElement('img')
oimg.src = '../vinter.png'
//  图片加载完毕
oimg.onload = () => {
    let imgObj = new fabric.Image(oimg, {
        //  图片属性
    })
}

```

**4. 图形渐变色**

```
rect.setGradient('fill', {
    //  位置
    x1: 0,
    y1: 0,
    x2: rect.width,
    y2: 0,
    
    //  渐变的颜色, 彩虹颜色
    colorStops:{
        0: 'red',
        0.2: 'orange',
        0.4: 'yellow',
        0.6: 'green',
        0.7: 'cyan',
        0.8: 'blue',
        1.0: 'purple'
    }
})
```

**5. 组合**

```
    let canvas = new fabric.Canvas('canvas');
    //绘制圆形
    let circle = new fabric.Circle({
        radius: 100,
        fill: '#eef',
        scaleY: 0.5,
        originX: 'center',//调整中心点的X轴坐标
        originY: 'center'//调整中心点的Y轴坐标
    });
    //绘制文本
    let text = new fabric.Text('Hello World', {
        fontSize: 30,
        originX: 'center',
        originY: 'center'
    })
    //进行组合
    let group = new fabric.Group([circle, text], {
        left: 150,
        top: 100,
        angle: 10,
        hoverCursor: 'pointer', //  鼠标悬停样式
        selectable: false,   //  不能选择
        hasControls: false,     //  只能移动 不能选择
        hasBorders: false,      //  去掉边框，可以操作
        hasRotatingPoint: false //  控制旋转点不可见
        //  ......
    })
    
    canvas.add(group);

```

**6. 设置动画**

三个属性：动画属性、发生变化的结束值、设置动画的细节属性。
```

//  方形逆时针旋转5度 持续2秒
rect.animate('angle', '-5', {
    onChange: canvas.renderAll.bind(canvas),
    duration: 2000,
    easing: fabric.util.ease.easeOutBounce
})

```

**7. 事件**

```
canvas.on({
    'touch:drag': function(){

    },
    'mouse:down': function(){

    },
    'mouse:up': function(){

    },
    'selection:created': function(){

    },
    'selection:update': function(){

    }
    //  ......

//  单个图形进行操作
    let rect = new fabric.Rect({
        //  ...
    })
    rect.on('selection:created', function(){

    })
})

```

**8. 序列化、反序列化、转化为SVG**

```
//  序列化
JSON.stringify(canvas.toJSON())

//  反序列化
canvas.loadFromJSON('字符串')

//  转化为SVG
canvas.toSVG()
```

## Fabric.js 内容列表

**1. 方法**

add(object) 添加
<br/>
insertAt(object,index) 添加
<br/>
remove(object) 移除
<br/>
forEachObject 循环遍历 
<br/>
getObjects() 获取所有对象
<br/>
item(int) 获取子项
<br/>
isEmpty() 判断是否空画板
<br/>
size() 画板元素个数
<br/>
contains(object) 查询是否包含某个元素
<br/>
fabric.util.cos
<br/>
fabric.util.sin
<br/>
fabric.util.drawDashedLine 绘制虚线
<br/>
getWidth()
<br/>
setWidth()
<br/>
getHeight()
<br/>
clear() 清空
<br/>
renderAll() 重绘
<br/>
requestRenderAll() 请求重新渲染
<br/>
rendercanvas() 重绘画板
<br/>
getCenter().top/left 获取中心坐标
<br/>
toDatalessJSON() 画板信息序列化成最小的json
<br/>
toJSON() 画板信息序列化成json
<br/>
moveTo(object,index) 移动
<br/>
dispose() 释放
<br/>
setCursor() 设置手势图标
<br/>
getSelectionContext()获取选中的context
<br/>
getSelectionElement()获取选中的元素
<br/>
getActiveObject() 获取选中的对象
<br/>
getActiveObjects() 获取选中的多个对象
<br/>
discardActiveObject()取消当前选中对象 
<br/>
isType() 图片的类型
<br/>
setColor(color) = canvas.set("full","");
<br/>
rotate() 设置旋转角度
<br/>
setCoords() 设置坐标


**2. 事件**

object:added
<br/>
object:removed
<br/>
object:modified
<br/>
object:rotating
<br/>
object:scaling
<br/>
object:moving
<br/>
object:selected 弃用
<br/>
before:selection:cleared
<br/>
selection:cleared
<br/>
selection:updated
<br/>
selection:created
<br/>
path:created
<br/>
mouse:down
<br/>
mouse:move
<br/>
mouse:up
<br/>
mouse:over
<br/>
mouse:out
<br/>
mouse:dblclick
<br/>


**3. 常用属性**

canvas.isDrawingMode = true; 可以自由绘制
<br/>
canvas.selectable = false; 控件不能被选择，不会被操作
<br/>
canvas.selection = true; 画板显示选中
<br/>
canvas.skipTargetFind = true; 整个画板元素不能被选中
<br/>
canvas.freeDrawingBrush.color = "#FFF" 设置自由绘画笔的颜色
<br/>
freeDrawingBrush.width 自由绘笔触宽度


**4. 文本方法**

selectAll() 选择全部
<br/>
getSelectedText() 获取选中的文本
<br/>
exitEditing() 退出编辑模式


**5. 对象属性**

obj.hasControls = false; 只能移动不能（编辑）操作
<br/>
obj.hasBorders = false; 去掉边框，可以正常操作
<br/>
hasRotatingPoint = false; 不能被旋转
<br/>
hasRotatingPoint 控制旋转点不可见

**6. 鼠标样式**

defaultCursor：默认光标，即鼠标在画布上的样式（默认值：default）
<br/>
hoverCursor：鼠标移动到对象上的样式（默认值：move）
<br/>
moveCursor：对象拖动时的鼠标样式（默认值：move）
<br/>
freeDrawingCursor：自由绘制时的鼠标样式（默认值：crosshair）
<br/>
rotationCursor：旋转时的鼠标样式（默认值：crosshair）  手型： pointer


> 合抱之木，生于毫末；九层之台，起于累土；千里之行，始于足下；
<br/>
> 越学习，越青春；


