### DOM
DOM是HTML和XML文档抽象的节点树，不仅仅js可以访问，其他语言如Python也可访问
#### Node节点
node节点是实现的文档节点对象
#### 常见类型
1. document 整个文档对象，window的属性
2. element 元素节点 
3. attr 属性节点
4. text 文本节点
5. comment 属性节点

#### 属性
1. nodeType 返回节点类型

类型 | nodeType
---|---
docunment| 9
element | 1
attr | 2
text | 3
comment | 8
2. nodeName 返回节点名称
3. nodeValue 返回节点的值，针对attr和text
4. 元素节点会有一个属性集合，即是元素标签内的大部分属性，如id，class，name，style等等，可以通过node.属性名访问，如document.getElementById('intro').style.color(直接访问是取值，后面加=号可以赋值)
注意：节点的style样式中带有-的属性，可以通个驼峰名访问，如：
```JavaScript
temp=document.getElementsByTagName("div") 
temp.style.backgroundColor="red"//设置样式的 background-color属性
```
##### 选取节点
```javascript
//1. 直接选中节点
//都是node对象的函数，如document.getElementsByClassName("class"),
//var temp=document.getElementsByClassName("class"),temp.getElementsByTagName("tag")
getElementById("id") //通过id来获取node节点，返回的是一个节点
getElementsByClassName("class")//通过class来获取node节点，返回的是一个数组
getElementsByTagName("tag")//通过tag来获取node节点，返回的是一个数组
//2. 相对节点选择
childNodes //获取所有子节点
firstChild //获取第一个子节点
lastChild  //获取最后i一个子节点
//子节点 可能是文本节点可能是元素节点
//注意：元素之间不能有空格，如果ul和li之间有空格的话，就会被认为是内容为空的text node节点
parentNode //获取父节点
nextSibing //获取下一个兄弟节点
previousSibing //获取前一个兄弟节点

querySelector(css选择符)//根据传入的css选择符查询，返回匹配的第一个元素，没有则返回null
querySelectorAll(css选择符)//根据传入的css选择符查询，返回匹配的所有元素，
//返回一个Nodelist对象，不是轮询的动态查询，是一个快照，不会存在性能问题
matchesSelector(css选择符)//检查元素是否满足传入的选择符，满足则返回true

childElementCount//返回子元素(不包括文本节点和注释)的个数
children//返回子元素
firstElementChild//返回第一个元素
lastElementChild//返回最后一个元素
previousElementSibing //返回前一个兄弟元素
nextElementSibing //返回后一个兄弟元素
contains()//a.contains(b)检查a元素是否包含b元素
```
#### 创建节点
```javascript
//分为两种，创建元素节点，创建文本节点
document.createElement('元素标签')

document.createTextNode('元素文本')//创建的文本节点加入元素节点，然后元素
//节点加入吉他元素节点，形成dom树结构

pnode.innerHtml('可以是完整的html文本')//将pnode内部的html全部替换成传入的
```


#### 添加节点
```javascript
pnode.appendChild(node)//将node添加到pnode的结尾
pnode.insertBefore(bullet, target)//将bullet添加到target前面
pnode.replaceChild(newnode, oldnode)//将newnode替换oldnode
pnode.removeChild(node)//在pnode上移除node

pnode.clonenode(true/false)//克隆pnode，如果参数为true，则会深度克隆pnoe并
//包括其子节点树，如果参数为false则只克隆该节点，克隆的节点没有挂接到
//document上，在html中不会显示，需要通过以上api来挂接
//注意，clone并不克隆pnode上的事件(如点击)等js属性
```
#### 访问属性
```javascript
pnode.getAttribute('属性名')//获取对应属性值，如id，title
pnode.getAttribute('属性名', 值)//设置对应属性的值
pnode.removeAttribute('属性名')//在pnode上移除对应属性
//注意 只有element元素可以
```
#### HTML5变化
```javascript
//元素的class属性新增一个继承了DOMTokenList的实例classList，是一个对象，
//以前是使用className返回字符串，通过操作字符串修改元素的class
DOMTokenList支持方法：
add(value)//添加value，已有则不操作
contains(value)//检查是否存在value
remove(value)//删除value
toggle(value)//如果存在value，则删除value，没有则添加value

document.activeElement//返回当前获取焦点的元素

document.readyState属性的值：
loadin//正在加载文档
complete//已经加载完文档

//滚动
element.scrollIntoView()//传boolean,表示元素是否滑动置顶，类似于hash锚点
```
### 事件
事件是指html上的交互行为，例如常见的点击
#### 事件冒泡/捕获/流
事件冒泡是指事件从产生交互的元素开始逐级向父级元素传递的过程，
事件捕获是指事件从包含触发交互元素的最上级父元素逐级向子元素捕获的过程
事件流包含三个阶段：事件捕获，处于目标阶段，事件冒泡
#### 事件处理
每个事件都会有一个对应的处理函数，如点击事件，对应处理函数就是onclick
```javascript
var myElement = document.getElementsByTagName('button');//第一步获取元素
//第二部指定事件处理函数

//1.直接为处理函数指定函数
myElement.onclick=function (){}
myElement.onclick=null//删除处理函数，及点击不会触发逻辑

//添加监听器
myElement.addEventListener('click',function (){},true/false)
//包含三个参数，第一个事件名，第二个对应处理函数，第三个true/false，
//对应捕获/冒泡阶段触发，建议false
myElement.removeEventListener('click',function (){},true/false)
//包含三个参数，第一个事件名，第二个对应处理函数(要和添加那里函数同名才能
//移除,匿名函数会失败)，第三个true/false，捕获/冒泡阶段触发
//IE
myElement.attachEvent('onclick',function (){})//相对前者，事件名是on+click
myElement.detachEvent('onclick',function (){})

//注意，IE的处理方式，函数的this指向全局对象，其他指向元素本身

//兼容
function addEvent(elem, type, fn) {
    if (elem.attachEvent) {
        elem.attachEvent('on' + type, fn);
    } else if (elem.addEventListener) {
        elem.addEventListener(type, fn, false);
    } else {
        elem['on' + type]=fn;
    }
}
function removeEvent(elem, type, fn) {
    if (elem.detachEvent) {
        elem.detachEvent('on' + type, fn);
    } else if (elem.removeEventListener) {
        elem.removeEventListener(type, fn, false);
    } else {
        elem['on' + type]=null;
    }
}
```
### Event对象
DOM中交互触发事件时，会产生一个Event对象，包含事件的有关信息，并会传到事件对应处理函数中；但在ie中需要通过window.event来获取   
1. 属性：  

target 事件的目标  (ie:srcElement) 

type 时间的类型，如click

detail 详细信息

defaultPrevented 是否禁止默认行为，如a标签跳转网页

bubbles 是否冒泡

cancelable 能否取消

eventPhase 当前时间阶段，1捕获，2处于目标，3冒泡
2. 方法：   

stopPropagation() 取消冒泡或捕获(ie:cancelBubble)

preventDefault() 禁止事件默认行为(ie:returnValue=false)






