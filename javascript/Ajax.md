Ajax依赖核心的是XMLHttpRequest对象，能够异步向服务器发送请求，可以在不刷新页面的情况下和后端通信
##### 创建XHR(XMLHttpRequest）
一般可以直接使用构造方法实现
```javascript
var xhr=new XMLHttpRequest()
```
但是IE7之前的版本有三个特殊版本，最兼容版本而下
```javascript
function makeXHR(){
    var xhr=null;  
    if (window.XMLHttpRequest)  
    {// 兼容 IE7+, Firefox, Chrome, Opera, Safari  
        xhr=new XMLHttpRequest();  
    } else {// 兼容 IE6, IE5 
        var versions=['MSXML2.XMLHTTP.6.0','MSXML2.XMLHTTP.3.0','MSXML2.XMLHTTP']
        for (var i=0;i<versions.length;i++){
            try{
                xhr=new ActiveXObject(versions[i]);
                break
            } catch(ex){
                console.log(versions[i]+' 不支持')
            }
        }
    }
    if(xhr==null){
        console.log('浏览器不支持ajax')
    }
}
```
##### 方法和属性
方法
```javascript
open(请求类型,url,是否异步)//设置请求类型，url，是否异步
send(data)//发送设置好的请求,data是要传递的数据，没有则传入null
abort()//终止异步请求，终止后不能访问属性

setRequestHeader(headername，headervalue)//设置自定义请求头，需在send调用之前执行
getResponseHeader(headername)//获取指定名字的请求头信息
getAllResponseHeader()//获取所有的请求头信息
```
属性

readystate，标识当前请求/响应的阶段
取值 | 状态
---|---
0 | 未初始化，即未调用open函数
1 | 启动，已调用open，但未调用send函数
2 | 发送，已调用send函数，但尚未接收到响应
3 | 接收，已接受部分响应数据，表明服务器已响应
4 | 完成，已接收全部数据
readystate的值每次切换都会触发readystatechange事件，需要open函数调用前设置好readystatechange事件
```javascript
var xhr=makeXHR();
xhr.addEventListener('readystatechange',function (){
    if (xhr.readystate==4 && xhr.status==200){
        console.log('请求返回数据了')
    }
})
```
##### XHR对象的属性

属性名 | 功能
---|---
status| http请求的状态码
statusText| 状态码说明
responseText| 响应返回的主体文本
responseXML| 响应返回的XML Dom文档，需要响应类型是text/xml或application/xml
timeout| 设置请求最大时长,当超出该时间时会触发timeout事件

##### FormData
FormData用于序列化表单或表单格式类似的数据
```javascript
var data=new FormData()
data.append(name,value)//往FormData中加入键值对

var data=new FormData(document.forms[0])//创建FormData对象时直接传入form表单对象

xhr.send(data)//可以直接给send发送出去
```
##### 同源策略影响
ajax请求受到同源策略影响，具体跨域请参考[跨域解决方法](https://github.com/ConstancePeng/readingNotes/blob/master/html/%E8%B7%A8%E5%9F%9F%E8%A7%A3%E5%86%B3%E6%96%B9%E6%B3%95.md)