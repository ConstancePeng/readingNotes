## cookie
cookie是针对http协议的无状态策略

#### 组成：

名字，不区分大小写

值，键值对字符串，需要通过url编码，encodeURIComponent()

域，domain，限制cookie发送到哪些域

路径，path，限制cookie发送到哪些路径，path和domain组成url共同决定是否发送cookie

失效时间，expire，决定cookie失效时间，失效的cookie会被浏览器删除，默认是会话结束(浏览器关闭)删除，

注意：expire在http1中生效，http1.1中用Maxage

安全标志：secure，只有https会发送cookie

http， 设置cookie是否能通过 js 去访问,true则表示只能HTTP访问，浏览器不能访问，默认false

#### cookie的操作
1. 服务器可以直接使用set-cookie进行设置，然后返回个客户端，客户端自动更新cookie
2. 客户端：
document.cookie,自动根据document的url获取有的cookie的键值对，设置属性会放到所有cookie中；
修改cookie即操作document.cookie获取到的字符串
#### cookie全过程
1. 浏览器请求服务器，首次不带cookie
2. 服务器判断是否带cookie，通过set-cookie，设置cookie信息，返回个浏览器
3. 浏览器接到响应，发现有set-cookie请求头，更新/创建cookie
4. 浏览器再次请求时，联通cookie消息发送给服务器
#### 限制：
1. 个数:每个域下cookie个数，不同浏览器不同
2. 大小：单个cookie的大小，不同浏览器不同
3. 替换：超过个数/无效时的替换规则，不同浏览器不同

#### cookie的缺陷：
1. 大小、数量限制
2. 每次请求都会发送，占领带宽
3. 不安全，cookie可能被转发

## session
htpp是不保存状态的协议，
seesion是一个键值对数据结构，是服务端记录用户状态的方案，cookie是客户端的方案
1. 当浏览器向服务器发送请求时，服务器会检查本次请求是否带有session标识及sessionid，如果没有就会创建一个，服务器会检查服务器存储中sessionid对应的session，获取其中记录的信息；
2. 服务器传递sessionid的策略有很多，可以通过cookie发送给浏览器，也可以加载url的后面
3. 服务器session的存储，可以存在数据库中，缓存中都可，和其他的数据存储一样，但是在分布式服务器布局中，就存在一个问题，同一个请求可能被随机分配到不同的服务器上，造成session查找不到，可以使session存储同一个数据库服务器，或者让指定的用户访问相同服务器，不被分配到其他服务器