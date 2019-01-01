transform实际上是对目标元素进行物理上变化，如移动，缩放，旋转，拉伸等

## 配置属性
1. transform-origin  
- 功能 指定元素变形的中心点，默认是元素的正中心，即想x/y/在50%，z在0的位置
- 取值 x/y可以设置具体长度(px)或百分比或top/right/bottom/left/center，z只能取具体px，如
```javascript
transform-origin: center；
transform-origin: 50% 10% 3px；
transform-origin: left top
```
具体取值影响如下图：
![image](https://note.youdao.com/favicon.ico)

注意：transform-origin对translate变化不产生影响，translate位移始终相对元素正中心

2. transform-style
- 功能 指定是2d还是3d，在父元素上设置
- 取值 默认flat，flat和preserve-3d对应2d和3d，具体就是z变化会不会展开

注意：overflow：hidden会使preserve-3d效果失效；在父元素上设置即可，子元素就展示3d效果
3. perspective
- 功能 指定3d的视距即眼睛距离屏幕的距离，默认none
- 取值 只能设置具体的px，越小表示眼睛离屏幕越近

4. perspective-origin
- 功能 设置视距的基点，也就是把眼睛放在哪，默认值是50% 50%即center
- 取值 只能设置x/y，可以设置具体长度(px)或百分比或top/right/bottom/left/center

5. backface-visibility
- 功能 设置是否可以看见目标的背面
- 取值 设置成hidden即不可见

常见HTML结构
```HTML
<舞台>        //为舞台加上perspective
    <容器>     //为容器加上preserve-3d，使容器内元素共享同一个3D渲染环境
        <元素> //为元素加上transform效果
    </容器>
</舞台>
```
## 常见方法
#### 2D
包含translate位移，scale缩放，rotate旋转，skew扭曲，matrix矩阵
##### translate
- 功能 移动目标元素，沿x/y移动，
- 取值 具体的px，只传入一个值时相当于只移动x，两个值是x，y都移动
```css
transform：translate（100px）//x正方向移动100px
transform：translate（100px，100px）//x，y正方向移动100px
```
可以具体拆分成两个单方向移动的属性：translateX，translateY

##### scale
- 功能 对目标进行缩放
- 取值 具体值，无单位，小数表示缩小，大于1的表示放大，默认1，一个值，x/y方向都生效，两个值，x/y各自变化
```css
transform：sacle（.5）//x,y都缩小为一半
transform：translate（1，.5）//y方向缩小为一半
```
可以具体拆分成两个单方向变化的属性：scaleX，scaleY
##### rotate
- 功能 对目标进行旋转
- 取值 具体值，单位为deg，正值顺时针旋转
```css
transform：rotate（50deg）//顺时针旋转50度
```
##### skew
- 功能 对目标进行扭曲
- 取值 具体值，单位为deg，只传入一个值时相当于只x扭曲，两个值是x，y都扭曲
```css
transform：skew（50deg）//x扭曲50度
transform：translate（50deg，100deg）//x，y方向分别扭曲
```
可以具体拆分成两个单方向变化的属性：skewX，skewY
##### matrix
矩阵变化，2D得变化都可以通过matrix来实现

### 3D
1. translate3d(tx,ty,tz)，三个方向位移，tz只能取实际值px

新增translateZ，控制Z轴移动
2. scale3d(sx,sy,sz)

新增scaleZ，控制Z轴缩放
3. rotate3d(x,y,z,a)

参数a（读音是阿尔法…）表示3D舞台上旋转的角度，而xyz的取值为0～1为各轴的旋转矢量值

新增rotateZ，控制Z轴旋转

matrix3d 三位转换矩阵

##### matrix/matrix3d
matrix包含6个参数，matrix(a,b,c,d,e,f),对应线性代数中的矩阵如下
```math
\begin{bmatrix}
a&c&e\\
b&d&f\\
0&0&1
\end{bmatrix}
```
注意参数顺序，matrix对原图形的生效过程就是进行线性代数运算
```math
\begin{bmatrix}
a&c&e\\
b&d&f\\
0&0&1
\end{bmatrix}
*
\begin{bmatrix}
x\\
y\\
1
\end{bmatrix}
=
\begin{bmatrix}
ax+cy+e\\
bx+dy+f\\
1
\end{bmatrix}


\begin{cases}
ax+cy+e=x'\\
bx+dy+f=y'
\end{cases}
```
最后根据矩阵的各个数据计算出行的x,y;

我们来看常见的2D变化如何使用矩阵实现
1. translate进行位移变化，就是说x，y互不影响，比例不变化，所以，a=d=1,c=b=0,最后剩下e，f分别表示移动距离
```math
\begin{cases}
ax+cy+e=x+e\\
bx+dy+f=y+f
\end{cases}
```
如matrix(1,0,0,1,10,20)=translate(10,20)

2. scale进行比例变化，就是说x，y互不影响，比例变化，位置不变化，所以c=b=e=f=0,最后剩下a，d分别表示缩放比例。
```math
\begin{cases}
ax+cy+e=ax\\
bx+dy+f=dy
\end{cases}
```
如matrix(2,0,0,0.1,0,0)=translate(2,0.1)
3. rotate进行旋转，可以拆解成x，y方向上的位移变化

![image](https://note.youdao.com/favicon.ico)
```math
\begin{cases}
x'=x*\cos\theta-y*\sin\theta\\
y'=x*\sin\theta+y*\cos\theta
\end{cases}
```
所以matrix(cos(a), sin(a), -sin(a), cos(a), 0, 0)=translate(2,0.1)rotate(a)
4. skew进行扭曲，可以拆解成x，y方向上的位移变化
```math
\begin{cases}
x'=x+y*\tan\theta x\\
y'=x*\tan\theta y+y
\end{cases}
```
matrix(1,tan(θy),tan(θx),1,0,0)=skew(θxdeg，θydeg)

matrix3D包含16个参数，matrix(a1,a2,a3,a4,b1,b2,b3,b4,c1,c2,c3,c4,d1,d2,d3,d4)，对应线性代数中的矩阵如下
```math
\begin{bmatrix}
a1&b1&c1&d1\\
a2&b2&c2&d2\\
a3&b3&c3&d3\\
a4&b4&c4&d4
\end{bmatrix}
```
如scale3d(sx, sy, sz)对应
```math
\begin{bmatrix}
sx&0&0&0\\
0&sy&0&0\\
0&0&sz&0\\
0&0&0&1
\end{bmatrix}
```

更高级应用待定