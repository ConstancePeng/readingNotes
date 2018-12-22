### 原生拖放

##### 设置可拖放
默认 图片、链接和文本是可以拖放的，其他的可以通过设置属性draggable，true表示可以，如
```HTML
<button draggable="true" id="tutorial">按钮</button>
```
##### 事件
###### 被拖元素
事件 | 功能
---|---
dragstart | 当元素刚被拖动时触发
drag | 元素拖动过程一直持续触发，类似于movesover
dragend | 当放开鼠标，停止拖动时出发
###### 目标元素
事件 | 功能
---|---
dragenter | 当有元素被拖动到目标元素时触发
dragover | 当被拖动元素一直在目标元素内时触发,当离开目标元素时就不触发
dragleave | 当被拖动元素离开目标元素时就触发
drop | 当被拖动元素被放到目标元素里时就触发
> 注意，所有元素都支持放置目标事件，但是默认是不允许放置的，再不允许放置的的元素上是不能触发drop事件的，通过重写这些元素的dragenter和dragover事件可以修改，如
```javascript
<div id='aa'></div>
aa.addEventListener('dragenter',function (e){
	e.preventDefault()
})
aa.addEventListener('dragover',function (e){
	e.preventDefault()
})
//此时就允许放置，并可以触发drop事件
```
##### 数据
event(事件)的属性dataTransfer可以传递数据

方法 | 功能
---|---
setData | 设置传递数据，两个参数，第一个是key，第二个是值
getData | 获取传递数据，一个参数，即key获取对应的值
clearData | 删除某个数据，一个参数
如
```javascript
var button = document.getElementById('tutorial');
var aa = document.getElementById('aa');
button.addEventListener('dragstart',function (e){
	e.dataTransfer.setData('target',e.target.id)//设置target的值
})
aa.addEventListener('drop',function (e){
	e.preventDefault()
	var d=e.dataTransfer.getData('target')//获取key为target的对应值
	console.log(d)
})
```
##### 允许的操作
```javascript
dropEffect//被拖动元素能执行哪种动作，none(不能放置)，move(移动到此处)，copy(复制到此处)，link(打开元素链接)
effectAllowed//与dropEffect配合使用，表示只允许对应的dropEffect操作
//取值uninitialized，none，copy，link，move，copylink，copymove，linkmove，all
```
> 注意：这些设置实际没有什么用，主要是来设置拖拉时鼠标的显示图标，实际并不会对元素操作，如移动元素，要在事件中处理才行。还有一个api可以设置鼠标图片:   
setTragImage(element,x,y)，设置拖动时的鼠标图片，x，y表示鼠标在图片中的位置，element可以是图片也可以是其他元素，图片的话则直接显示，元素的话显示渲染之后的元素
