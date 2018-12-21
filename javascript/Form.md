### Form
表单元素是具有html元素的默认属性外，还有以下特殊属性：
```JavaScript
action 请求的url
elements 表单中所有元素的集合
length 表单中空间的个数
method 发送http请求的类型，一般为get或post
name 表单的名字
reset() 将表单重置为默认值
submit() 提交表单

document.forms 获取所有的的表单，可以通过name或顺序获得对应表单对象
document.forms[0]//第一个表单
document.forms['test']//name为test的表单
```
#### 提交
1. 在表单内加入按钮，type属性设置为submit，为按钮绑定提交事件，则按钮点击时提交表格
2. 获取form元素，使用submit()提交

重置与提交相同，事件为reset

#### 表单元素
```JavaScript
document.forms 获取所有的的表单，可以通过name或顺序获得对应表单对象
document.forms[0].elements[0]//第一个表单的第一个元素
document.forms[0].elements['test']//第一个表单的name为test的元素

表单元素的属性、方法、事件：
disabled 元素是否可用
form 所属表单对象
name 元素name属性值
type 类型，如checkbox
value 当前字段被提交服务器的值

focus() 当前元素获取焦点
blur()  当前元素失去焦点

blur 事件，当前元素失去焦点时触发
focus 事件，当前元素获取焦点时触发
change <select>选项改变时触发，<input>,<textarea> 失去焦点且value改变时触发
```
#### 输入验证
在提交函数里验证，如限制字符类型，输入长度
```HTML
require 属性，该元素为必填项
type 取值email，url，number限制输入类型

checkValidity() 检查字段是否有效，每个表单元素都有，有效则返回true
```
