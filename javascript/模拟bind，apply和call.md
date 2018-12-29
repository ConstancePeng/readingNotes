## call
先看call是怎么使用的
```javascript
var obj={
    foo:1
}
function test(){
    console.log(this.foo)
}
test.call(obj)//1
```
可以看到call改变了test函数内的this指向，this指向了obj，我们都知道obj内部this指向自己，常见的调用方式如下
```javascript
var obj={
    foo:1,l
    test:function (){
    console.log(this.foo)
    }
}
obj.test()//1
```
发现两种方式的结果一样，也就是说call可以转化成作用域内的函数调用，步骤如下
1. 在作用域内新建属性指向函数
2. 通过作用域调用函数
3. 删除添加在作用上的属性
```javascript
Object.prototype.mycall=function(context){
    context=context || window//可能不传也可能传null，defined
    context.fn=this//fn.mycall(context)此时，this指向函数本身，为作用域新增属性指向函数
    context.fn()
    delete context.fn
}
```
如果有参数，怎么处理，call的参数是一个一个放在参数里的，个数不定，使用arguments处理
```javascript
Object.prototype.mycall=function(context){
    //args=Array.prototype.slice.call(argument,1)
    context=context || window//可能不传也可能传null，defined
    var args=[]
    for (var i=1;i<arguments.length;i++){//第一个参数是context
        args.push('arguments['+i+']')
    }
    context.fn=this//fn.mycall(context)此时，this指向函数本身，为作用域新增属性指向函数
    var result=eval('context.fn('+args+')')//数组+字符串会自动转化为字符串相加
    //'context.fn('+args+')'转化为'context.fn(arguments[1],arguments[2],arguments[3],arguments[4])'
    delete context.fn
    return result
}
```
也可以用es6新功能实现
```javascript
Object.prototype.mycall=function(context){
    context=context || window//可能不传也可能传null，defined
    var args=[]
    for (var i=1;i<arguments.length;i++){//第一个参数是context
        args.push('arguments['+i+']')
    }
    context.fn=this//fn.mycall(context)此时，this指向函数本身，为作用域新增属性指向函数
    var result=context.fn(...args)//...arg转化为参数序列
    delete context.fn
    return result
}
```
主要记忆点：
1. 利用给传入的作用域添加属性指向函数模拟作用域直接调用函数，然后再删除属性
2. 函数执行call时，其内this指向函数自己，fn.call,this指向.左边的对象
3. 处理参数利用arguments，利用eval指向函数+参数的字符串
## apply
与call类似，传入的参数只有两个
```javascript
Object.prototype.myapply=function(context){
    context=context || window//可能不传也可能传null，defined
    context.fn=this //fn.mycall(context)此时，this指向函数本身，为作用域新增属性指向函数
    var args=arguments[1]//第一个是context，第二个是参数数组
    if (!args) {
        result = context.fn(); 
    } 
    else { 
        if (!(args instanceof Array)) throw new Error('params must be array'); 
        result = eval('context.fn('+args+')'); 
    }
    delete context.fn
    return result
}
```
## bind
bind的主要功能是为函数调用指定this，常见使用方法
```javascript
var obj={
    foo:1
}
function test(){
    console.log(this.foo)
}
var temp=test.bind(obj)
temp()//1
//函数有直接调用还有new
```
可以发现，bind绑定作用域以后返回了一个函数，比较类似于柯里化的方案，使用闭包记录第一次输入参数，返回一个执行函数
```javascript
Object.prototype.mybind=function(context){
    if (typeof this !=='function') throw new TypeError('绑定的不是函数') 
    var args=Array.prototype.slice.call(arguments,1)
    var that=this //因为返回闭包，需要记录复函数的this，也就是父函数自身
    
    var newfunc= function (){
        //决定返回的函数作用域是context 还是this，
        //当使用new调用时，this指向了实例对象，此时就用实例对象作为作用域
        var innercontext= this instanceof newfunc ? this : context
        //内部函数的参数，即第二次调用时传入的参数
        var innerargs=Array.prototype.slice.call(arguments)
        
        that.apply(innercontext,args.concat(innerargs))        
    }
    
    //当对返回的函数执行new 操作，真的bind是将绑定前的函数作为构造函数，
    //所以这里在父函数中将原函数的原型赋给返回函数的原型，让new操作能够创建和原函数一样的实例
    
    //方法1
    //var temp=function (){}
    //temp.prototype=this.prototype
    //newfunc.prototype= new temp()
    
    //方法2
    newfunc.prototype= this.prototype//测试证明，该方法才能使new出来的实例一模一样
    
    return newfunc
}
```
主要记忆点:
1. 主要逻辑是利用闭包记录绑定时传入的作用域、参数和函数本身，然后返回一个函数，其内部将父函数使用apply，指定作用域、参数(包括第二次调用时传入的参数)，整个过程很类似于柯里化
2. 函数绑定作用域以后，有种调用方式，一种是直接调用，一种是作为构造函数使用new创建对象，此时函数的作用于应该自动指向新产生的对象，所以在返回的函数中要判断作用域，方法是判断当前this是指向函数本身还是函数的一个实例；为了使返回函数new出来的对象和原函数创建的一样，需要将原函数的原型赋给返回的函数的原型

测试

```
Object.prototype.mybind=function(context){
if (typeof this !=='function') throw new TypeError('绑定的不是函数') 
    var args=Array.prototype.slice.call(arguments,1)
    var that=this
    var newfunc= function (){
        var innercontext= this instanceof newfunc ? this : context
        var innerargs=Array.prototype.slice.call(arguments)
        that.apply(innercontext,args.concat(innerargs))        
    }
    var temp=function (){}
    temp.prototype=this.prototype
    newfunc.prototype= new temp()
    return newfunc
}
Object.prototype.mybind2=function(context){
if (typeof this !=='function') throw new TypeError('绑定的不是函数') 
    var args=Array.prototype.slice.call(arguments,1)
    var that=this
    var newfunc= function (){
        var innercontext= this instanceof newfunc ? this : context
        var innerargs=Array.prototype.slice.call(arguments)
        that.apply(innercontext,args.concat(innerargs))        
    }
    newfunc.prototype= this.prototype
    return newfunc
}
var obj={
    foo:1
}
function test(a,b){
	this.foo=2
    console.log(this.foo)
}
test.prototype={
	foo:4
}
var a=test.mybind(obj,1,2,3,4)
var b=test.mybind2(obj,1,2,3,4)
console.log(new a())
/*
{…}
  foo: 2
  <prototype>: {}
     <prototype>: {…}//主要区别在这里
       foo: 4
       <prototype>: Object { mycall: mycall()
       , myapply: myapply(), mybind: mybind(), … }
*/
console.log(new b())
/*
{…}
  foo: 2
  <prototype>: {…}
    foo: 4
    <prototype>: Object { mycall: mycall()
    , myapply: myapply(), mybind: mybind()
    , … }
*/
console.log(new test())
/*
{…}
  foo: 2
  <prototype>: {…}
    foo: 4
    <prototype>: Object { mycall: mycall()
    , myapply: myapply(), mybind: mybind(), … }
*/
```
