BOM是浏览器对象模型，不用的浏览器会略有差异
### window
1. 在浏览器中，window对象充当全局对象，是js程序的入口
2. frames

在HTML文档中可以定义frameset集合和设置多个frame，每个frame都会有自己的window对象，可以用top(始终指向浏览器)来访问每个frame，如top.frames[0]
3. 窗口位置
```javascript
window.screen

window.screenX
window.screenY
window.screenLeft
window.screenTop
//窗口相对于屏幕左边和上边的位置，可以判断screenLeft是否有值来选择screenLeft还是screenX来兼容浏览器
window.moveBy//移动多少
window.moveTo//移动到哪里
//都是两个值表示水平和垂直的值
```
4. 窗口大小
```javascript
window.innerWidth
window.innerHeight

window.outerHeight
window.outerWidth

//窗口改变多少
window.resizeBy
//窗口调整到多少，都包含水平和垂直值
window.resizeTo
```
5. 打开窗口
```javascript
window.open(url,窗口名,“窗口设置”,boolean)
window.open(url,"_blank")
```
6. 定时相关
```javascript
setTimeout(function，time(ms))//设定一段时间以后执行，只执行一次，返回一个定时id，可以用clearTimeout(id)取消定时任务
setInterval(function，time(ms))//设定间隔一段时间执行，会一直执行，返回一个定时id，可以用clearInterval(id)取消间隔定时任务
```
7. 系统对话框
```javascript
alert(string)//弹出对话框，显示sting和一个ok框，会阻断代码执行
confirm(string)//弹出确认对话框，显示sting和一个ok和cance框，会阻断代码执行，根据选择不同返回true和false。
prompt(string1,string2)//弹出输入对话框，显示sting1和一个ok和cance和输入框(默认string2)，会阻断代码执行，根据选择不同返回输入的值和null。
```
### location
location是window和document的属性,window.location和document.location是引用同一个对象
```javascript
location.hash 返回url的hash(#号后面的，没有则返回空)//可以用来跨域传递信息
location.host 返回域名和端口
location.hostname 返回域名
location.href 返回完整url
location.pathname 返回url中的目录和文件
location.port 返回域端口
location.protocol 返回协议
location.search 返回url的查询字符串(如"?x=asd")
```
location.assign(url)，在当前窗口加载url，同时会在浏览器历史中生成记录。设置window.location=url和location.href=url与assign一样。修改location的属性也可以改变当前加载页面。如hash，search，hostname，pathname和port。

location.replace(url)，也会在当前页面加载新url，但是不会历史纪录，点击后退不会返回前一个页面

location.reload(true)，重新加载当前页面，传入true会强制从服务器重新加载，不传会以高效方式加载，如利用缓存。


