- [<script>标签](https://github.com/ConstancePeng/readingNotes/blob/master/javascript/javascript%E5%9F%BA%E7%A1%80.md#%E6%95%B0%E6%8D%AE%E7%B1%BB%E5%9E%8B) 
- [数据类型](https://github.com/ConstancePeng/readingNotes/blob/master/javascript/javascript%E5%9F%BA%E7%A1%80.md#%E6%95%B0%E6%8D%AE%E7%B1%BB%E5%9E%8B) 
   - [五种基础数据](https://github.com/ConstancePeng/readingNotes/blob/master/javascript/javascript%E5%9F%BA%E7%A1%80.md#) 
   - [引用数据类型](https://github.com/ConstancePeng/readingNotes/blob/master/javascript/javascript%E5%9F%BA%E7%A1%80.md#) 
      - [Array(数组)](https://github.com/ConstancePeng/readingNotes/blob/master/javascript/javascript%E5%9F%BA%E7%A1%80.md#) 
      - [Date(日期)](https://github.com/ConstancePeng/readingNotes/blob/master/javascript/javascript%E5%9F%BA%E7%A1%80.md#) 
      - [RegExp(正则表达式)](https://github.com/ConstancePeng/readingNotes/blob/master/javascript/javascript%E5%9F%BA%E7%A1%80.md#)
      - [Function](https://github.com/ConstancePeng/readingNotes/blob/master/javascript/javascript%E5%9F%BA%E7%A1%80.md#)
- [typeof](https://github.com/ConstancePeng/readingNotes/blob/master/javascript/javascript%E5%9F%BA%E7%A1%80.md#typeof) 
- [其他](https://github.com/ConstancePeng/readingNotes/blob/master/javascript/javascript%E5%9F%BA%E7%A1%80.md#%E5%85%B6%E4%BB%96) 
### <script>标签  
作用是在html页面上加载javascript
1.  async 立即下载脚本的同时不妨碍页面的其他操作
2. defer 可以等到文档解析完成以后执行脚本
3. src 脚本的下载路径
4. type 指定脚本语言类型，一般是text/javascript
### 数据类型
1. #### 五种基础数据  Undefined Null Boolean Number String
   1.  *空字符串，0，NaN，null，和undefined可以转成false;其他都可以转成true*  
   2. *数值存在最小和最大值，超过返回-Infinity和+Infinity，可以使用isFinity()函数判断*  
   3. *数值有一个特殊值，非数值，NaN，用作运算结果返回为非数值的情况，可以使用isNaN()函数判断,NaN的所有操作均返回NaN，NaN与任何值不相等包括自身*  
   4. *parseInt(字符，基数)将字符串转成整数，忽略前面的空格，直到找到第一个非空字符，如果第一个字符不是数字或负号，则返回NaN,如果第一个字符是数字，则会继续解析直到末尾或第一个非数字符，使用时最好先传一个基数；parseFloat()和parseInt()相同，解析时会忽略所有前导零，只有第一个小数点生效*    
   5. *字符串一旦创建就不可变，拼接字符串是返回新的字符串给变量，如var a='s',a=a+'dd'*  
2. #### 引用数据类型  Object(无序键值对)  
   1. ##### Array(数组): 可以同时存储任意数据类型
      1. ###### 创建方式  
      一种是使用构造函数，可以指定长度num，也可以指定几个包含的项；也可以不使用new创建,如 var arr=new Array(num/items) ， var arr=Array(5)（new被省略，效果一样）， var arr=Array(5,'asda','dddd');</br>   另一种是使用字面量创建，如var arr=[item1,item2....],使用字面量创建时，不要在最后一个元素后加逗号，可能会导致数组长度在不同浏览器中长度不一样;</br>  创建数组时会根据元素的长度设置数组的长度，但数组的长度是会改变且可以设置的，所以可以通个改变长度来删除元素或加大数组长度，如a=[1,3,5,6],a.length=2,a就变成[1,2]，当设置数组长度较大时，不足的元素会用undefined填充
      2. ###### 方法：
         1. ###### 增/删/改/查：
         可以直接用下标来设置元素，未填充的下标对应元素用undefined填充，如a=[1,2]；		
a[4]=3,则a--> [1,2,undefined,3]   
push(item)在数组的最后添加一个元素   
unshift()在数组的最前添加一个或多个元素   
pop()删除数组的最后一个元素，并返回该元素   
shift()删除数组的最前一个元素，并返回该元素  
         2. ###### 转换：  
         join(分隔符)，用指定分隔符拼接数组，如果数组元素是非string，调用toString()转化为字符串拼接，如果有多层数组，也用此方式拼接内部数组再向外拼接。
         3. ###### 检查：   
         a instanceof Array 判断a是否为数组,可能会在不同库中间结果不同Array.isArray(a)判断a是否为数组 EMCScript5新增方法
         4. ###### 排序：  
         sort()，将数组元素转变成字符串进行比较。如果要自定义比较规则可以传入一个比较函数，比较函数接受两个参数，如果第一个参数在第二个参数前面则返回一个负数，相等则返回0，第一个参数在第二参数后面则返回一个正数。   
reverse(),将数组的元素顺序颠倒。
         5. ###### 位置：   
         indexOf()/lastIndexOf()均接受两个参数，查找的元素和开始查找的位置(可选)，不同的是indexOf()从前往后差查找，lastIndexOf()正好相反，从后往前。
         6. ###### 迭代：对数组的每一项运行对应函数，参数都是一个函数和一个指定域，函数包含三个参数，数组项的值，数组项的位置和数组本身；对原数组没有影响。   
         every，对数组的每一项结果都返回true，该函数才返回true；   
some，对数组的任意一项结果都返回true，该函数就返回true；   
foreach,对数组的每一项执行，无返回；   
filter，返回结果为true的数组所有元素的新数组；   
map，返回每次运行的结果组成的数组。   
         7. ###### 归并：   
         reduce/reduceRight，迭代数组的所有项，最终返回一个值，reduce从第一项开始，reduceRight从最后一项开始。</br>两者都接受2个参数：一个每一项都调用的函数和一个作为归并的起始值；调用的函数包含4个参数，前一个结果，当前值，项的索引和数组对象，这个函数返回的结果会作为第一个参数传给数组的下一项。
         8. ###### 其他：   
            1. concat(),参数可以是数组和元素，会在原数组基础上生成新数组，如a.concat('asda',['aa','ddd']),返回一个在a后面添加参数里各项的新数组，a=[1,'ee'],则返回[1,'ee','asda','aa','ddd']。   
            2. slice(),将数据切片并返回新数组，不会改变原数组；参数可以为1个或2个，2个参数表示返回从起始位置到结束位置的数组元素，但不含结束位置的元素，1个参数表示从起始位置到最后的所有元素。   
            3. splice()，参数为1个或1个以上，第一个参数表示针对数组操作的起始位置（当只有一个参数时，可以看作是从该位置开始，元素全部删除，因为第二个参数和后面的参数都没有，即全部被删除），第二个参数表示要删除的元素个数，后面的若有参数则从起始位置开始插入对应参数元素。如：splice(1,3),从第二个元素开始删除3个元素；splice(1,3，‘aaa','ssss','dddd'),和前面的一样，删除元素后从第二个元素开始依次插入‘aaa','ssss','dddd'；splice(1,0，‘aaa','ssss','dddd')，因为第二个参数是0，所以不删除元素，直接从第二个元素开始依次插入‘aaa','ssss','dddd'。
   2. ###### Date(日期)：  
      1. 静态函数：  
parse()，参数为字符串，将字符串转化为相应日期的毫秒数，支持：年/(,-)月/日，月/日/(,-)年等，在IE中'2016-8-12'不能识别，'2016-08-12'才能。   
UTC()参数分别为年、月、日、时、分、秒、毫秒、时区，年月为必须参数，月从0开始，天从1开始；返回对应的毫秒数。   
      2. 构造函数：
      new Date()：没有参数，获取当前时间，当参数为字符串时，底层调用的时parse，当参数为年月日时分秒等时，底层调用的是UTC，参数也可以是毫秒数，返回对应时间对象。
      3. 注意：   
         1. new Date(Date.parse('2016-8-12'))和Date.parse('2016-08-12')在不同浏览器中获取到的时间略有差异：FF中两者相同，chrome中，后者比前者多8个小时，在IE总中，前者返回NaN   
         2. parse和UTC返回的都是数字，可以直接加减，也可以传入到构造函数中返回Date对象。   
         3. 可以使用+获取Date对象的时间戳，如+new Date()
      4. 对象函数：   
to类型：输出对应的日期字符串  
toString/toLocalString/toTimeString等   
get/set类型：获取/设置日期对应的参数   
getTime/setTime；getSecends/setSecends等   
   3. ###### RegExp(正则表达式)：
      1. 创建方式：   
字面量：/pattern/flags，pattern是正则表达式，flags标记正则表达式的行为：g:匹配所有字符串，而不是匹配到第一项时就停止，i:忽略大小写，m：多行匹配。   
构造函数：new RegExp(pattern,flags),两个参数都是字符串   
注意：3版本时字面量始终共享同一个RegExp实例，而构造函数每次都会创建新的RegExp实例，新的5版本已修正。
      2. 实例属性：   
ingoreCase/global/multiline/source/lastIndex
前四个是创建时初始化的属性，source为正则表达式的规则，字面量和构造函数创建的实例的source属性相同。
      3. 实例方法：   
         1. exec，参数为要执行匹配的字符串，返回包含第一个匹配项的数组，未匹配到则返回null。返回的数组是Array的实例，但包含额外的index和input属性，index表示匹配项再字符串中的起始位置，input为输入的字符串即应用匹配的字符串。在返回的数组，第一项是与整个模式匹配到的字符串，其他项是与模式中捕获组匹配到的字符串（无捕获组则只有一项）。捕获组是指正则中用小括号括起来的项如a（b（c）），捕获组就有bc，c。   
      注意：   
      如果模式是全局的，exec每执行一次只会匹配一次，匹配到的字符串起始位置记录在返回的数组的index属性中，结束位置记录在模式的lastindex中，下次执行时在上一次结束的位置继续执行，直到匹配到末尾，当上一次执行到了结尾时，再执行会从开始的的位置重新匹配。
         2. test，参数为要执行匹配的字符串，如果能匹配到则返回true，否则false。
当实例不是全局匹配时，以上两个方法每次执行结果一样，当实例是全局匹配时，以上两个方法每执行一次，返回一个匹配结果，再执行时，在前一次的基础上，继续向后执行，实例属性中的lastIndex记录当前匹配到的位置，返回结果的index记录开始匹配的位置。
         3.toString()和toLocalString()都返回正则表达式的字面量。
compile可以重新指定正则实例的规则与修饰符。例如：var pattern = /res/i；pattern.compile('rp','g') -> /rp/g
      4. 静态属性：
         1. input/lastMatch(最近一次匹配项)/lastParen(最近一次匹配的捕获组)/leftContext/rightContext/mutiline
支持正则的字符串方法：
         2. match，参数是一个正则表达式或RegExp实例，返回的数组和RegExp实例的exec方法一摸一样。
         3. search，参数与match相同，返回字符串第一个匹配项的索引，没有则返回-1；search和其他正则的方法不一样，不是在全局会从上次匹配的地方继续往下执行，search始终都是从字符串的开始位置向后匹配。
         4. replace，参数为两个，第一个为一个字符串或一个RegExp实例，如果是字符串，只会替换第一个匹配到的，若是RegExp实例(全局的)，则会替换整个字符串。第二个参数可以是一个字符串，也可以是一个函数。
         5. split，参数可以是一个字符串或RegExp实例，返回分割后的字符串数组，可选参数是数字长度，确保数组大小。
      5. 字符串的正则方法：   
先按照正则表达式匹配(flags为g时能匹配几次就执行几次，不为g则只执行一次)，当正则表达式中有分组时，每次匹配时记录分组数据。
   4. ###### Function
      1. 创建方式：   
         1. 声明：function a(){}   
         2. 表达式：var a=function (){};  
         3. 构造函数：var a=new Function('num1','num2,'return num1+num2'); // 不推荐   
声明和表达式基本相同，构造函数不推荐使用，会造成解析两次代码。由于函数也是一个对象，函数名只是是指向这个对象的指针(或地址)，所以函数可以有多个名字，都可以执行，函数名返回函数地址，函数名加()才会执行。
注意：js中没有重载，当定义多个相同名字的函数时，函数会被覆盖，相当于函数名的值被修改。
      2. 实例属性：
         1. length，返回函数定义的参数数量，如function a(c,d){}，则a.length返回2
         2. prototype，返回函数原型
      3. 实例方法：
         1. apply，参数有两个，第一个为函数的作用域(对应函数内部的this值)，第二个是参数数组，参数数组可以是Array的实例，也可以是arguments对象。
         2. call，参数与apply相似，第一个为作用域，只是传给函数的参数是分别给的，有几个就传几个。call和apply函数的作用相同，都是调用函数，只是传参数方式不同，apply直接传参数数组，而call需要逐个列出。
         3. bind，参数只有一个，给函数绑定作用域。
      4. 内部属性：
         1. arguments，是一个类数组对象，包含了传给函数的所有参数，可能比在函数定义中定义的形参数量多，还有一个callee属性，指向拥有arguments对象的函数，返回函数本身，可以用来解耦递归。
         2. this，指向作用于对象。
         3. caller，返回调用函数的对象，可以使用arguments.callee.caller来查看调用者。
### typeof
> 获取数据类型的操作符，使用方式 typeof 变量  
> Undefined->undefined、Boolean->boolean、Number->number、String->string、Object—>object  
> 注意 Null返回object 函数返回function





### 其他
> 1.递增和递减都是等包含他们的语句被执行完之后再执行的，例如：
>```
>var a=10,b=1;
>var c=a-- + b; // 11
>var d=a + b; //10
>//原因就是a-- + b;先按照a,b原来的值10,1来计算，所以c为11，执行完以后a递减为9，所以d为10
>```
> 2.\==、！=和===、!==  
> &nbsp;&nbsp; 1).== 和!= 会先转换比较的对象，再进行比较，转换规则:  
> &nbsp;&nbsp;*字符串、布尔型数据、数值的比较都会转换成数值进行比较,对象调用valueof比较*  
>```
>'3'==true//false
>'1'==true//true
>'3'==1//false
>'3'==3//true
>2==true//false
>1==true//false
>```
> &nbsp;&nbsp; 2).===和!\== 不会转换比较对象，即数据类型也要比较
