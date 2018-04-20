---
title: JavaScript基础-对象-实践对象构造
date: 2018-04-20 14:48:40
tags: JavaScript
categories: Web
---

实现弹跳球如下图所示:		
![bouncing-balls](bouncing-balls.png)		
一、准备基础代码	
	
```html
<head>
  <meta charset="UTF-8">
  <title>对象构造实践</title>
  <link rel="stylesheet" href="js-demo.css">
</head>
<body>
  
  <h1>bouncing balls</h1>
  <canvas></canvas>



  <script src="js-demo.js"></script>
</body>
```	
```css
html, body {
  margin: 0;
}

html {
  font-family: 'Helvetica Neue', Helvetica, Arial, sans-serif;
  height: 100%;
}

body {
  overflow: hidden;
  height: inherit;
}

h1 {
  font-size: 2rem;
  letter-spacing: -1px;
  position: absolute;
  margin: 0;
  top: -4px;
  right: 5px;

  color: transparent;
  text-shadow: 0 0 4px white;
}
```
```js
// setup canvas

var canvas = document.querySelector('canvas');

//获得一个开始画画的环境,申明一个对象变量作为绘制2D图像的区域
var ctx = canvas.getContext('2d');

//设置画布的宽高等于浏览器的宽高
var width = canvas.width = window.innerWidth;
var height = canvas.height = window.innerHeight;

//生成一个(min,max)范围内的随机数
function random(min,max) {

  var num = Math.floor(Math.random()*(max-min)) + min;
  return num;
}
```
二、构造函数 - 小球模型化		
1.构造器- 小球模型化		
```js
//构造器 - 小球模型化
function Ball(x,y,velX,velY,color,size){
  //位置：浏览器左上角（0,0）
  this.x = x;
  this.y = y;

  //速度 x,y方向
  this.velX = velX;
  this.velY = velY;
  this.color = color;
  //以像素为单位的大小 - 半径
  this.size = size;
}
```
2.构造器方法:
画小球

```js
//方法1：画小球
Ball.prototype.draw = function(){

    ctx.beginPath();
    ctx.fillStyle = this.color;//定义这个形状的颜色
    //画圆弧arc(坐标x,坐标y,半径,开始角度,结束角度)
    ctx.arc(this.x,this.y,this.size,0,2 * Math.PI);
    //结束绘制，并以fillStyle颜色填充
    ctx.fill();
}
```
更新小球	

```js
//方法2:更新小球数据
Ball.prototype.update = function(){
  //检测小球是否碰到边缘，碰到则让小球反向移动

    //小球超出或在浏览器右边界
    if((this.x + this.size) >= width){
        this.velX = -(this.velX);
    }
    //小球超出或在左边界
    if((this.x - this.size) <= 0){
        this.velX = -(this.velX);
    }
    //小球超出或在下边界
    if((this.y + this.size) >= height){

        this.velY = - (this.velY);
    }
    //小球超出或在上边界
    if((this.y - this.size) <= 0){
        this.velY = -(this.velY);
    }

    this.x += this.velX;
    this.y += this.velY;
}
```

三、渲染小球		
1.浏览器显示静态小球	

```js
//创建小球实例
var testBall = new Ball(50,100,4,4,'blue',10);

//浏览器上渲染处蓝色小球
testBall.draw();
```

2.小球动起来		
几乎所有的动画效果都会用到一个运动循环，也就是每一帧都自动更新视图。这是大多数游戏或者其他类似项目的基础.		

```js
1.创建存放所有小球的容器数组:
var balls = [];

2.创建loop循环:
function loop(){

    //设置画布颜色和位置大小
    ctx.fillStyle = 'rgba(0,0,0,0.25)';
    ctx.fillRect(0,0,width,height);

    while(balls.length < 25){

      var ball = new Ball(random(0,width),
                          random(0,height),
                          random(-7,7),
                          random(-7,7),
                          'rgb(' + random(0,255) + ', ' + random(0,255) + ' ,' + random(0,255) +')',
                          random(10,20));
      balls.push(ball);
    }
    for(var i = 0; i < balls.length; i ++){

      balls[i].draw();
      balls[i].update();
    }

    /*使用 requestAnimationFrame() 方法再运行一次loop函数 — 
    当一个函数正在运行时传递相同的函数名，从而每隔一小段时间都会运行一次这个函数，从而得到一个平滑的动画效果。
    这主要是通过递归完成的 — 也就是说函数每次运行的时候都会调用自己，从而可以一遍又一遍得运行
    */
    requestAnimationFrame(loop);
}

3.动起来
loop();

```
四、增加撞击侦查		

```js
//方法3:增加撞击侦查
Ball.prototype.collisionDetect = function(){

    for(var i = 0; i < balls.length; i++){

		//碰撞小球不是自己
        if(!(this === balls[i])){
			 
            var dx = this.x - balls[i].x;
            var dy = this.y - balls[i].y;

            //两球圆心的距离 < 两球半径只和 - 相撞
            var distance = Math.sqrt(dx * dx + dy * dy);
				
            if(distance < this.size + balls[i].size){

                balls[i].color = this.color = 'rgb(' + random(0,255) + ', ' + random(0,255) + ' ,' + random(0,255) +')';
            }
        }
    }
}

//在loop的for 循环中update下加入:
ball[i].collisionDetect();

```






参考资料：	
1.[对象](https://developer.mozilla.org/zh-CN/docs/Learn/JavaScript/Objects)		   
2.[JS中的实践对象构造](https://developer.mozilla.org/zh-CN/docs/Learn/JavaScript/Objects/Object_building_practice) 		
3.[Canvas_API](https://developer.mozilla.org/en-US/docs/Learn/JavaScript/Client-side_web_APIs/Drawing_graphics)	 
4.[requestAnimationFrame API](https://developer.mozilla.org/en-US/docs/Web/API/window/requestAnimationFrame)		
5.[一个很好的2D游戏开发初学者教程](https://developer.mozilla.org/en-US/docs/Games/Tutorials/2D_Breakout_game_pure_JavaScript)		
6.[ JavaScript游戏库构建2D游戏的基础知识](https://developer.mozilla.org/en-US/docs/Games/Tutorials/2D_breakout_game_Phaser)		
7.[](https://developer.mozilla.org/zh-CN/docs/Learn/JavaScript/Objects/%E5%90%91%E2%80%9C%E5%BC%B9%E8%B7%B3%E7%90%83%E2%80%9D%E6%BC%94%E7%A4%BA%E7%A8%8B%E5%BA%8F%E6%B7%BB%E5%8A%A0%E6%96%B0%E5%8A%9F%E8%83%BD)