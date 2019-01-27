#### 类定义
```Python
class classname(parentClasss):#可以继承多个类
    '''class doucumentation string'''
    static_property_name=value#静态(类)属性，通过classname.static_property_name访问
    
    #创建实例时调用的个方法，会将参数传入
    #a=classname(1,2,k=2),会调用 __init__，并将1,2,k=2等参数传入
    def __init__(self,*args,**kargs)
        parentClass.__init__(self,*args,**kargs)#父类显示调用初始化
        #可以使用super(classname,self)指向父类，如super(classname,self).__init__(*args,**kargs)
        self.propertynmae=value#实例属性赋值，只能通过实例访问
    
    #实例方法，通过实例访问a.func()
    def funcname(self,*args,**kargs):#self指向实例本身
        code here
    
    #静态(类)方法，通过类访问classname.staticfunc(),不可以访问类内部属性
    @staticmethod
    def staticfunc(*args,**kargs)
        code here
    
    #静态(类)方法，通过类访问classname.staticfunc()，可以访问类内部属性
    @classmethod
    def staticfunc(*args,**kargs)
        code here
 ```
1. 类属性属于类本身，与该类的任何实例无关，且这些实例都可以共享
2. 在类内部访问静态方法要通过classname.static_property_name
3. 类和实例有相同属性时，实例会访问实例属性，实例中的属性被删除，此时实例访问的是类属性
```python
class Foo(object):
    x=1.5
foo=Foo()
foo.x#1.5,访问类属性，实力没有此属性
foo.x=7
foo.x#7
Foo.x#1.5,foo实例不能修改类属性，上面的操作是给实例添加了一个实例属性
```


#### 内置方法
##### 类方法
方法名 | 功能
---|---
\_\_dict__ | 包括类的属性和方法，以及__doc__,\_\_module__
\_\_doc__ | 类内定义的文档
\_\_name__ | 类的名字
\_\_bases__ | 类的所有父类构成的元组
\_\_module__ | 类定义所在的模块
\_\_doc__ | 类内定义的文档
\_\_del__ | 类的实例的所有引用都被清除是调用，可以用于释放资源等清除工作
##### 实例方法
方法名 | 功能
---|---
\_\_class__ | 实例是由哪个类创建的
\_\_dict__ | 实例的属性

#### 继承
基本格式
```python
class subClassname(parentClass...):
    pass
```
与通常的类继承相同，子类会获得父类的属性和方法

python类可以继承多个父类，那如何查找父类方法和属性呢？

I. 新式类是通过C3算法实现，会生成一个查找列表L(classname.mro()可以获取到)，按照L中的先后顺序查找属性和方法，基本规则是
1. 从最基层的父类计算查询列表L
2. 每个继承的子类计算方式是L[C(P1,P2,..,PN)]=C+merge[L(P1),L(p2),...,L(PN),P1P2P3..PN]
3. merge的原则是从merger的第一个中开始取出第一个类，如L(P1)[0],如果不是其他任何中的的非第一个(即只要存在于其他的L中，就是第一个)，则取出放到merge的外面，并从其他里面删除；如果不满足，则跳过，继续取出从第二个中去除第一个类，如L(P1)[0],循环执行，直到都取出，如果都不满足而且还有剩余则报tb
```python
O = object
class F(O): pass
class E(O): pass
class D(O): pass
class C(D,F): pass
class B(D,E): pass
class A(B,C): pass

L[O]=O
L[D]=D+merge(L[O],O)=DO
L[E]=EO
L[F]=FO
L[B(D,E)]=B+merge(L[D],L[E],DE)
    =B+merge(DO,EO,DE)
    =BD+merge(O,EO,E)
    =BDE+merge(O,O)
    =BDEO
L[C(D,F)]=C+merge(L[D],L[F],DF)
    =C+merge(DO,FO,DF)
    =CD+merge(O,FO,F)
    =CDF+merge(O,O)
    =CDFO
L[A(B,C)]=A+merge(L[B],L[C],BC)
    =A+merge(BDEO,CDFO,BC)#先在BDEO取B，B只要出现(BDEO,BC)就是第一个
    =AB+merge(DEO,CDFO,C)#继续BDEO中取D,但是在CDFO中不是第一个，向后从CDFO中取C，B只要出现(CDFO,C)就是第一个
    =ABC+merge(DEO,DFO)#从DEO中取D,D满足
    =ABCD+merge(EO,FO)#依次取E,F,O
    =ABCDEFO
    
O = object
class X(O): pass
class Y(O): pass
class A(X,Y): pass
class B(Y,X): pass
class C(A,B): pass
L[X]=XO
L[Y]=YO
L[A]=AXYO
L[B]=BYXO
L[C]=C+merge(AXYO,BYXO,AB)
    =CA+merge(XYO,BYXO,B)
    =CAB+merge(XYO,YXO)#此时X,Y都不满足c3算法，解释器会报tb，应尽力避免
```
4. 获得查询列表L后，在调用子类或其实例的属性或方法时，会按照L中类的排序查找，查找到一个时立即返回
```python
class A(object):
	a=1

class B(object):
	a=2

class C(object):
	c=3

class D(A,B):
	c=4
	
class E(C,A):
	pass

class F(D,E):
	pass

#F的L是FDECAB
print F.a#1
print F.c#4
print F.mro()#FDECABo
```
II. 经典类使用深度优先查找，即在一个父类上没找到时，去父类的父类上查找，一直没找到，再从另一个父类上查找
```python
class A:#经典类，最终继承自type，新式类写成class A(object):
	a=1

class B(A):#继承自A也变成经典类
	pass

class C(A):
	a=2
class D(B,C):
	pass

print D.a#1,从B开查找，到B的父类A查找

class A(object):#新式类，继承自object
	a=1

class B(A):#继承自A也变成经典类
	pass

class C(A):
	a=2
class D(B,C):
	pass
	
print D.a#2,查找列表L为DBCA
```

子类调用父类方法可以使用super(subClassname,self).parentFuncName，但是只会查找第一个父类
```python
class A(object):
	def __init__(self,a):
		print 'A'
		self.a=a

class B(object):
	def __init__(self,a):
		print 'B'
		self.b=a

class C(B,A):
	def __init__(self,a):
		super(C,self).__init__(a)#只调用了B的 __init__

d=C(1)
print d.b#1
print d.a#exception

class C(B,A):
	def __init__(self,a):
	    A.__init__(self,a)
	    B.__init__(self,a)#显示分别调用A,B的 __init__

d=C(1)
print d.b#1
print d.a#1
```
#### 关系判断方法
```python
issubclass(sub,sup)#判断sub是不是sup的子类，注意sub和sup是一个类时也返回True

isinstance(obj1,obj2)#判断obj1是否是obj2的实例或其子类的实例

hasattr(obj,attr)#obj是否含有attr属性
getattr(obj,attr)#获取obj的attr属性
setattr(obj,attr,val)#设置obj的attr属性的值为val
delattr(obj,attr)#删除obj的attr属性
```