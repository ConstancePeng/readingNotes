## 基础
canvas元素支持width，height设置，及其他默认设置，可以应用css样式
如：
```HTML
<canvas id="tutorial" width="150" height="150"></canvas>
注意：当没有设置宽度和高度的时候，canvas会初始化宽度为300像素和高度为150像素
```
通过getContext获取上下文，其接受一个参数，传入'2d'/'3d'即可画2d/3d图形，如：
```JavaScript
var canvas = document.getElementById('tutorial');
if (canvas.getContext){
  var ctx = canvas.getContext('2d');
  // drawing code here
} else {
  // canvas-unsupported code here
}
注意：要检查浏览器是否支持canvas，(canvas.getContext)

toDataURL(类型)//导出canvas的图片，可以出如图片的MIME类型，如image/png
```
## 2D
### 2D图形
2d图形操作分为两种：填充和描边，填充是将图形内部填满颜色，描边是在图形边缘画线。对应的属性:strokeStyle,fillStyle，都是设置颜色的；绘制不同2d图形的api都支持两种模式，一般为strokexxx,fillxxx，对应选取strokeStyle,fillStyle设置的颜色，默认黑色
##### 矩形
```javascript
fillRect(x, y, width, height)//绘制一个填充的矩形
strokeRect(x, y, width, height)//绘制一个矩形的边框
clearRect(x, y, width, height)//清除指定矩形区域，让清除部分完全透明。
```
##### 移动笔触
```javascript
moveTo(x, y)//将笔触移动到指定的坐标x以及y上
```
##### 线
```javascript
lineTo(x, y)//绘制一条从当前位置到指定x以及y位置的直线。
```
> 注意：路径使用填充（filled）时，路径自动闭合，使用描边（stroked）则不会闭合路径。如果没有添加闭合路径closePath()到描述三角形函数中，则只绘制了两条线段，并不是一个完整的三角形。

##### 圆弧
```javascript
arc(x, y, radius, startAngle, endAngle, anticlockwise)
//画一个以（x,y）为圆心的以radius为半径的圆弧（圆），
//从startAngle开始到endAngle结束，按照anticlockwise给定的方向（默认为顺时针）来生成。

arcTo(x1, y1, x2, y2, radius)
//根据给定的控制点和半径画一段圆弧，再以直线连接两个控制点
```
> 注意：arc()函数中的角度单位是弧度，不是度数。角度与弧度的js表达式:radians=(Math.PI/180)*degrees。

##### 贝塞尔曲线
```javascript
quadraticCurveTo(cp1x, cp1y, x, y)
//绘制二次贝塞尔曲线，cp1x,cp1y为一个控制点，x,y为结束点。
bezierCurveTo(cp1x, cp1y, cp2x, cp2y, x, y)
//绘制三次贝塞尔曲线，cp1x,cp1y为控制点一，cp2x,cp2y为控制点二，x,y为结束点
```
>  注意：这些划线操作最终都要调用stroke或fill来画到canvas上，如
```javascript
var canvas = document.getElementById('tutorial');

if (canvas.getContext){
	var a=function(){
	  var ctx = canvas.getContext('2d');
	  ctx.beginPath();
	  ctx.fillStyle='red';
	  ctx.arc(10+i*10,100,10,0,2*Math.PI,false);
	  ctx.fill(); //不调用则在canvas上看不到任何图片
	  i+=1;
	}
	var s = setInterval(a,1000) 
} else {
  console.log(11)
}
```
### 2D文字
```javascript
fillText(text,x, y, 可选最大像素宽度)//绘制一个填充的字
strokeText(text,x, y, 可选最大像素宽度)//绘制一描边的字

measureText(text)//返回TextMetrics对象，TextMetrics.width可以获得文本的宽度
属性
font//设置文字样式，大小和字体，如bold 14px Arial
textAlign//文本对齐方式，如center，start，end，left，right
textBaseLine//文本基线，如top，middle，bottom
```
例如
```javascrip
if (canvas.getContext){
  var ctx = canvas.getContext('2d');
  ctx.font="bold 52px Arial"
  ctx.strokeText('32',100,100);
} else {
  console.log(11)
}
```
### 变换
```javascript
rotate(angle)//围绕原点旋转angle度
scale(scaleX,scaleY)//缩放图形，x,y方向分别乘以scaleX,scaleY，默认是1
translate(x,y)//把坐标原点移动到x,y
transform(m1-1,m1-2,m2-1,m2-2,dx,dy)//修改矩阵，直接乘以
//                                    m1-1 m1-2 dx
//                                    m2-1 m2-2 dy
//                                    0     0    1
setTransform(m1-1,m1-2,m2-1,m2-2,dx,dy)//设置矩阵然后调用transform

save()//将此时的设置保存到堆栈中，多次执行会逐次压入堆栈
restore()//从堆栈中恢复save()保存的设置记录，多次执行会逐次从堆栈中弹出
```

### 其他
```javascript
beginPath()//表示开始绘制新路径。
closePath()//表示关闭绘制路径。

//绘制图像
drawImage(image,sourex,sourey,sourexsize,soureysize,destinyx,destinyy,destinyxsize,destinyysize)
//将原图像x-y为起点，宽高分别为sourexsize,soureysize的图像 绘制到 destinyx,destinyy处，宽高变为destinyxsize,destinyysize

//当参数为三或五时都是针对目标位置生效
drawImage(image,x,y)//图片绘制到 x，y处，全图绘制不做变化
drawImage(image,x,y,xsize,ysize)//图片绘制到 x，y处，全图绘制，大小变化为xsize,ysize

//绘制阴影
shadowColor//阴影颜色，默认黑色
shadowOffsetX//x方向阴影，默认0
shadowOffsetY//y方向阴影，默认0
shadowBlur //模糊像素，默认0

//渐变
是CanvasGradient的实例
createLinearGradient(startX,startY,endX,endY)//创建线性渐变
createRadialGradient(startX,startY,startradius,endX,endY,endradius)//创建线性渐变

addColorStop(step,color)//设置渐变颜色,step是颜色位置，0是起点，1是终点

//渐变要赋值给strokeStyle,fillStyle才能实际对图形生效

//模式
createPattern(image，style)//设置区域内图片重复模式，repeat，repeat-x，repeat-y，no-repeat
//渐变要赋值给strokeStyle,fillStyle才能实际对图形生效，此时不会绘制指定形状的线，而是在区域内绘制模式指定的图片
```