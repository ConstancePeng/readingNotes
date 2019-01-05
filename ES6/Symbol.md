# Symbol 
用来标识变量唯一，在为模块、对象等添加属性时，对避免覆盖已有属性很有效，感觉有点类似于MD5
#### 创建
1. 直接调用函数Symbol，每一个Symbol都是不同的，是一个原始数据类型，和number，string等一样
```javascript
let m=Symbol();
let n=Symbol();

console.log(m===n)//false
```
2. 可以传入字符串作为显示使用，对象也会通过toString转换成字符串
```javascript
let a=Symbol('a');
let b=Symbol('b');
console.log(a)//Symbol(a)
console.log(b)//Symbol(b)

const obj = {
  toString() {
    return 'abc';
  }
};
const sym = Symbol(obj);
sym // Symbol(abc)
```
3. Symbol不能与其它类型的值进行运算，可以转换成字符串，布尔，但不转换成数值
```javascript
let sym = Symbol('My symbol');

"your symbol is " + sym
// TypeError: can't convert symbol to string
`your symbol is ${sym}`
// TypeError: can't convert symbol to string
```
#### 作为属性
每个Symbol都是不同的，作为属性标识符不会出现覆盖
1. 标识
```javascript
let mySymbol = Symbol();

// 第一种写法
let a = {};
a[mySymbol] = 'Hello!';

// 第二种写法
let a = {
  [mySymbol]: 'Hello!'//注意对象字面量写法需要加[]
};

// 第三种写法
let a = {};
Object.defineProperty(a, mySymbol, { value: 'Hello!' });

// 以上写法都得到同样结果
a[mySymbol] // "Hello!"
```
2. 访问
只能通过[Symbol]访问，不能用.访问
```javascript
a[mySymbol]
```
3. 遍历
- Symbol 作为属性名，该属性不会出现在for...in、for...of循环中，也不会被Object.keys()、Object.getOwnPropertyNames()、JSON.stringify()返回；
- Object.getOwnPropertySymbols方法，可以获取指定对象的所有 Symbol 属性名
- Reflect.ownKeys方法可以返回所有类型的键名，包括常规键名和 Symbol 键名


#### 常用API
1. Symbol.for

类似于单例模式，会查找传入的字符串的Symbol是否存在，存在则返回，不窜再则创建返回
```javascript
let s1 = Symbol.for('a');
let s2 = Symbol.for('a');

s1 === s2 // true
```
2. Symbol.keyFor

返回Symbol传入的字符串，当Symbol传入的字符串已被注册，则相当于没传，返回undefined
```javascript
let s1 = Symbol.for("foo");
Symbol.keyFor(s1) // "foo"

let s2 = Symbol("foo");
Symbol.keyFor(s2) // undefined
```