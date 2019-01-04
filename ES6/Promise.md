# Promise
#### 基本介绍
Promise是ES6定义的一个实现异步避免回调地狱的构造函数，在浏览器中结构如下

![image](https://github.com/ConstancePeng/readingNotes/blob/master/pic/Promise.png)

包含all，resolve，reject和race函数，原型prototype中包含catch，finally，then函数，constructor指向Promise，其中最关键的是resolve，reject，then和catch这四个，后面依次介绍


#### 创建实例
1. Promise限制了只能通过new创建实例对象，不能直接调用(和vue一样)
2. Promise实例化时传入的参数必须是函数，函数参数在执行时会自动传入Promise的resolve，reject，可以将参数设置为resolve，reject获取函数，方便后面执行(也可以通过arguments获取)
```javascript
var a=new Promise(function(resolve,reject){
    console.log(arguments);
})
```
![image](https://github.com/ConstancePeng/readingNotes/blob/master/pic/promise(func).png)

3. 在实例完Promise后，传入Promise的函数会立刻执行,但不会阻塞后面代码的执行，而是异步去执行传入Promise的函数。
```javascript
var a=new Promise(function(resolve,reject){
    console.log(11);
})
//会立刻打印11
consle.log(a)
```
#### then
上面介绍到Promise实例化后，传入构造函数的函数会立刻开始异步执行，那现在就有一个问题，该如何获取到这个函数执行结果呢?这里就要用到then，不过需要在传如构造函数的函数里进行配合
1. 在构造函数里，显示调用resolve或reject传递数据或异常
2. 调用then函数，传入两个(第二个不强求)需要执行的回调函数，第一个是fulfilled状态执行，第二个是rejected状态执行，函调函数的参数即是用resolve或reject传递数据或异常
3.then函数中的回调函数只有当Promise实例状态发生变化时才会执行
4. then函数中的回调函数中返回的对象就是then的返回对象，可以在其中在返回一个Promise实例，可以以链式方式实现回调嵌套
```javascript
var p=new Promise(function(resolve,reject){
    setTimeout(() => {
        Math.random() > 0.5 ? resolve('success') : reject('fail');
    }, 1000)
})

p.then((result) => {
    console.log(result);//success
}, (err) => {
    console.log(err);//fail，和success只会有一个执行
});

//then的回调函数返回一个新的promise对象给t
var t =p.then((result) => {
    return new Promise(function(resolve,reject){
		setTimeout(() => {
			Math.random() > 0.5 ? resolve('success') : reject('fail');
		}, 1000)
	});
}, (err) => {
   return err;
});

t.then((result) => {
    console.log(result);//success
}, (err) => {
    console.log(err);//fail
});
```
通过上面的例子，我们可以理解Promise的作用是实现串联函数(传入其构造函数的函数)与其回调(传入then的函数)，

这里需要解释一下，promise实例有一个状态，刚开始创建时，值为pending(执行中)，即传入的构造函数正在执行
1. 只有调用resolve或reject才能改变其状态值，不会受到其他的影响；resolve修改状态为fulfilled(成功)，reject修改状态为rejected(失败)
2. 状态的变化是不可逆的，即只能由pending变为fulfilled或rejected，不能变回到pending，fulfilled和rejected也不能互相变化
3. resolve或reject只有一个能执行，fulfilled和rejected状态只会出现一个。当Promise实例状态变化为fulfilled或rejected后，再调用then可以再次获取数据(即异步函数执行的结果)

#### catch
Promise.prototype.catch是.then(null,rejection)或.then(undefined, rejection)的别名，专门用于获取异常，在promise实例的状态变为rejected是执行
```javascript
var p=new Promise(function(resolve,reject){
    setTimeout(() => {
        Math.random() > 0.5 ? resolve('success') : reject('fail');
    }, 1000)
})

p.catch((err) => {
    console.log(err);
});
```
Promise 对象的错误具有“冒泡”性质，会一直向后传递，直到被捕获为止。也就是说，错误总是会被下一个catch语句捕获。

```javascript
var p=new Promise(function(resolve,reject){
    setTimeout(() => {
        Math.random() > 0.5 ? resolve('success') : reject('fail');
    }, 1000)
})

p.then((result) => {
    return new Promise(function(resolve,reject){
        setTimeout(() => {
            Math.random() > 0.5 ? resolve('success') : reject('fail');
        }, 1000)
})
}).then((result)=>{
    console.log(result);//success
}).catch((err) => {
    console.log(err);
});
```
上面代码中，一共有三个Promise对象：一个由new Promise产生，两个由then产生。它们之中任何一个抛出的错误，都会被最后一个catch捕获。
一般来说，不要在then方法里面定义 Reject 状态的回调函数（即then的第二个参数），总是使用catch方法。

注意:在node中如果promise实例化时调用了reject，而没有在then或catch中处理rejected状态的函数，会报一个UnhandledPromiseRejectionWarning；在Firefox浏览器中不会。

promise对象内部有异常，会直接将终止异步函数，而不会影响外部执行

#### finally
1. finally是Promise原型上的方法，表示不管promise对象最终状态如何都会执行的方法，不依赖于promise的结果和状态；
2. 一般在then或catch执行完后执行，类似于try...except...finally
3. finally的回调函数不接受任何参数
```javascript
promise
.then(result => {···})
.catch(error => {···})
.finally(() => {···});
```
#### all
Promise.all方法用于将多个 Promise 实例，包装成一个新的 Promise 实例
```javascript
const p = Promise.all([p1, p2, p3]);
```
上面代码中，Promise.all方法接受一个数组作为参数，p1、p2、p3都是 Promise 实例，如果不是，就会先调用下面讲到的Promise.resolve方法，将参数转为 Promise 实例，再进一步处理。（Promise.all方法的参数可以不是数组，但必须具有 Iterator 接口，且返回的每个成员都是 Promise 实例。）
p的状态由p1、p2、p3决定，分成两种情况。
1. 只有p1、p2、p3的状态都变成fulfilled，p的状态才会变成fulfilled，此时p1、p2、p3的返回值组成一个数组，传递给p的回调函数。
2. 只要p1、p2、p3之中有一个被rejected，p的状态就变成rejected，此时第一个被reject的实例的返回值，会传递给p的回调函数。

注意，如果作为参数的 Promise 实例，自己定义了catch方法，那么它一旦被rejected，并不会触发Promise.all()的catch方法。 

#### race
Promise.race方法同样是将多个 Promise 实例，包装成一个新的 Promise 实例
```javascript
const p = Promise..race([p1, p2, p3]);
```
上面代码中，只要p1、p2、p3之中有一个实例率先改变状态，p的状态就跟着改变。那个率先改变的 Promise 实例的返回值，就传递给p的回调函数。

race与all分别类似于或和与，race是或，任意一个promise状态变化，包裹的promise状态就变化，all是与，都为fulfilled返回fulfilled，有一个rejected返回rejected

#### resolve&reject
Promise.resolve和Promise.reject都可以将对象转化为promise对象

```
Promise.resolve('foo')
// 等价于
new Promise(resolve => resolve('foo'))

const p = Promise.reject('出错了');
// 等同于
const p = new Promise((resolve, reject) => reject('出错了'))
```
Promise.resolve方法的参数分成四种情况
1. 参数是一个 Promise 实例
Promise.resolve将不做任何修改、原封不动地返回这个实例。
2. 参数是一个thenable对象
Promise.resolve方法会将这个对象转为 Promise 对象，然后就立即执行thenable对象的then方法。
3. 参数不是具有then方法的对象，或根本就不是对象
如果参数是一个原始值，或者是一个不具有then方法的对象，则Promise.resolve方法返回一个新的 Promise 对象，状态为resolved。
4. 不带有任何参数
Promise.resolve方法允许调用时不带参数，直接返回一个resolved状态的 Promise 对象。

Promise.reject方法
1. Promise.reject(reason)方法也会返回一个新的 Promise 实例，该实例的状态为rejected。
2. Promise.reject()方法的参数，会原封不动地作为reject的理由，变成后续方法的参数
