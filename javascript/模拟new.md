先看new的常见用法
```
 function A(a,b,c){
     this.a=a;
     this.b=b;
     this.c=c;
 }
 var a=new A(1,2,3);
 a
 /*
 Object { a: 1, b: 2, c: 3 }
 */
```
javascript高级程序设计中写到，new操作做了四件事
> 1. 创建一个新对象；
> 2. 将构造函数的作用域赋给新对象(也就是this指向新对象)；
> 3. 执行构造函数的代码；
> 4. 返回新对象。

我们新建一个函数来模拟这个实现
```javascript
function mynew(){
   if (arguments.length<1 || typeof arguments[0] !== 'function' ) throw new Error('第一个参数请输入构造函数');
   //默认第一个参数是构造函数
    construnctor=arguments[0];
    var tempobj=new Object();//1.创建对象
    var args=Array.prototype.slice.call(arguments,1)
    tempobj.__proto__=construnctor.prototype//将新对象的原型指向构造函数的原型
    construnctor.apply(tempobj,args)//2,3 this指向和执行构造函数代码
    return tempobj//返回对象
}
```
我们测试一下
```
function A(a,b,c){
    this.a=a;
    this.b=b;
    this.c=c;
}
var a=new A(1,2,3);
var b=mynew(A,1,2,3)
console.log(a)
/*
object { a: 1, b: 2, c: 3 }
*/
console.log(b)
/*
object { a: 1, b: 2, c: 3 }
*/
```
结果相同，基本实现了，但是要注意一个特殊情况，当构造函数有自己的返回时，new操作符不会返回自己创建的对象，而是返回构造函数的返回，如
```
function A(a,b,c){
    this.a=a;
    this.b=b;
    this.c=c;
    return {d:1}
}
var a=new A(1,2,3);
var b=mynew(A,1,2,3)
console.log(a)
/*
Object { d: 1 }
*/
console.log(b)
/*
object { a: 1, b: 2, c: 3 }
*/
```
发现结果不一样，对代码做一下修改
```javascript
function mynew(){
   if (arguments.length<1 || typeof arguments[0] !== 'function' ) throw new Error('第一个参数请输入构造函数');
   //默认第一个参数是构造函数
    construnctor=arguments[0];
    var tempobj=new Object();//1.创建对象
    var args=Array.prototype.slice.call(arguments,1)
    tempobj.__proto__=construnctor.prototype//将新对象的原型指向构造函数的原型
    var result=construnctor.apply(tempobj,args)//2,3 this指向和执行构造函数代码
    return result//返回对象
}
```
测试一下
```
function A(a,b,c){
    this.a=a;
    this.b=b;
    this.c=c;
    return {d:1}
}
var a=new A(1,2,3);
var b=mynew(A,1,2,3)
console.log(a)
/*
Object { d: 1 }
*/
console.log(b)
/*
Object { d: 1 }
*/
```
成功！