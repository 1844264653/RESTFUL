### 我们必须要知道的RESTful服务最佳实践

###### 前言

~~~RESTful
看顾很多RESTful的文章总结，参差不齐，结合工作中的使用，我觉得非常有必要归纳一下关于RESTful架构方式了
RESTful只是一种架构方式的约束，给出了以一种约定的标准，完全严格遵守RESTful标准并不是很多，也没有必要
但是实际运用中，参考RESTful标准是非常有必要的
实际上在工作中api接口规范，命名规则，返回值，授权验证等进行一定的约束，一般的项目api只要容易测试，足够安全，风格一直，可读性强，没有歧义调用方便我觉得已经足够了
接口是给开发人员看的，也不是给普通用户去调用
~~~

**回想起刚工作，，并没有仔细的了解一些约束，给自己挖坑，给别人挖坑**

##### 导读

REST是什么，应该知道的6大原则

1. C-S架构

2. 无状态

3. 统一的接口

4. 一致的数据格式

5. 自我描述的信息

6. 超媒体即应用住状态引擎（HATEOAS）

RESTful使用时应注意的问题

1. 版本（Versioning）

2. 参数命名规范

3. url命名规范

4. 统一返回数据格式

5. http状态码

6. 合理使用query parameter

7. 多表、多参数连接查询如何设计URL

8. API请求授权

##### REST的来源

REST：Representational State Transfer——表象层状态转变——绝对不是rest这个单词啊喂大哥们

如何理解REST的晦涩难懂的架构，最好的办法就是深刻理解Representational State Transfer 这三个单词到底意味着什么。

​	1.每一个URL代表一种资源

​	2.客户端和服务器之间，传递这种资源的某种表现层

​	3.客户端通过四个http动词——get post put delete，对服务器端资源进行操作，实现“表象层状态转换”

由美国计算机科学家Roy  Fielding（我擦，百度百科没有啊？？）——Adobe首席科学家、http协议的首要作者置一，Apache项目联合创始人

2000年发布论文，没啥银关注——那时候web前后端还融合在一起，接口服务这些东东即使存在这种概念，都没地方发挥作用

但是好东西经得起时间考验，不信你看唐宋八大家几千年还在传承，郭敬明小说00后看的人多嘛？（我瞎说的，粉丝们不要搞我）

随着前后端分离的思想爆发开来——嘿嘿嘿，RESTful架构：老子总算熬出头了

**博客园的开发平台：https://developers.douban.com/wiki/?title=api_v2 
豆瓣API： https://developers.douban.com/wiki/?title=api_v2 
GitHub开放平台： https://developer.github.com/v3/ 
Roy Fielding的原中英文论文地址如下，可以收藏下载看看，论文一般都比较晦涩难懂。 
架构风格与基于网络的软件架构设计.pdf 
Architectural_Styles_and_the_Design_of_Network-based_Software_Architectures.pdf 
前人栽树，后人乘凉。关于RESTful服务最佳实践 总结的一篇非常好的文章也是外国人写的，原英文下载：RESTful Best Practices-v1 2.pdf 
有园友翻译总结后的中文：https://www.cnblogs.com/jaxu/p/7908111.html**

##### REST是什么？应该了解的6大原则

REST之父在论文中阐述6大原则——上面提到了，这里详细解释一下

######C-S架构 
  数据的存储在Server端，Client端只需使用就行。两端彻底分离的好处使client端代码的可移植性变强，Server端的拓展性变强。两端单独开发，互不干扰。 

###### 无状态 
  http请求本身就是无状态的，基于C-S架构，客户端的每一次请求带有充分的信息能够让服务端识别。请求所需的一些信息都包含在URL的查询参数、header、body，服务端能够根据请求的各种参数，无需保存客户端的状态，将响应正确返回给客户端。**无状态的特征大大提高的服务端的健壮性和可拓展性**。 
  当然这总无状态性的约束也是有缺点的，客户端的每一次请求都必须带上相同重复的信息确定自己的身份和状态（这也是必须的），造成传输数据的冗余性，但这种确定对于性能和使用来说，几乎是忽略不计的。 

######统一的接口 
  这个才是**REST架构的核心**，统一的接口对于RESTful服务非常重要。客户端只需要关注实现接口就可以，接口的可读性加强，使用人员方便调用。 
一致的数据格式 
  服务端返回的数据格式要么是XML、要么是Json（获取数据），或者直接返回状态码，有兴趣的可以看看博客园的开放平台的操作数据的api，post、put、patch都是返回的一个状态码 
  自我描述的信息 
  每项数据应该是可以自我描述的，方便代码去处理和解析其中的内容。比如通过HTTP返回的数据里面有 [MIME type ]信息，我们从MIME type里面可以知道数据的具体格式，是图片，视频还是JSON 

######超媒体即应用状态引擎（HATEOAS）* 
   客户端通过body内容、查询串参数、请求头和URI（资源名称）来传送状态。服务端通过body内容，响应码和响应头传送状态给客户端。这项技术被称为超媒体（或超文本链接）。 
   　　除了上述内容外，HATEOAS也意味着，必要的时候链接也可被包含在返回的body（或头部）中，以提供URI来检索对象本身或关联对象。下文将对此进行更详细的阐述。 
   　　如请求一条微博信息，服务端响应信息应该包含这条微博相关的其他URL，客户端可以进一步利用这些URL发起请求获取感兴趣的信息，再如分页可以从第一页的返回数据中获取下一页的URL也是基于这个原理 

######系统分层 
   客户端通常无法表明自己是直接还是间接与端服务器进行连接，分层时同样要考虑安全策略。 
######可缓存 
   　　在万维网上，客户端可以缓存页面的响应内容。因此响应都应隐式或显式的定义为可缓存的，**若不可缓存则要避免客户端在多次请求后用旧数据或脏数据来响应**。管理得当的缓存会部分地或完全地除去客户端和服务端之间的交互，进一步改善性能和延展性。 
######按需编码、可定制代码（可选） 
   服务端可选择临时给客户端下发一些功能代码让客户端来执行，从而定制和扩展客户端的某些功能。比如服务端可以返回一些 Javascript 代码让客户端执行，去实现某些特定的功能。 
   提示：REST架构中的设计准则中，只有按需编码为可选项。如果某个服务违反了其他任意一项准则，严格意思上不能称之为RESTful风格。



##### RESTful使用应该注意的问题

######版本（Versioning） 
​	如github开放平台 https://developer.github.com/v3/ ，就是讲版本放在url，简洁明了，这个只有用了才知道，一般的项目加版本v1，v2，v3?好吧，这个加版本估计只有大公司大项目才会去使用，说出来不怕尴尬，我真没用过。有的会将版本号放在header里面，但是不如url直接了当。

​		https://example.com/api/v1/

######参数命名规范 
​	query parameter可以采用驼峰命名法，也可以采用下划线命名的方式，推荐采用下划线命名的方式，据说后者比前者的识别度要高，可能是用的人多了吧，因人而异，因团队规范而异吧。

​		https://example.com/api/users/today_login 获取今天登陆的用户 
​		https://example.com/api/users/today_login&sort=login_desc 获取今天登陆的用户、登陆时间降序排列

######url命名规范 
​	API 命名应该采用约定俗成的方式，保持简洁明了， 在RESTful架构中，每个url代表一种资源所以url中不能有动词，只能有名词，并且名词中也应该使用复数。实现者应使用相应的Http动词GET、POST、PUT、PATCH、DELETE、HEAD来操作这些资源即可 
不规范的的url，冗余没有意义，形式不固定，不同的开发者还需要了解文档才能调用

​		https://example.com/api/getallUsers GET 获取所有用户 
​		https://example.com/api/getuser/1 GET 获取标识为1用户信息 
​		https://example.com/api/user/delete/1 GET/POST 删除标识为1用户信息 
​		https://example.com/api/updateUser/1 POST 更新标识为1用户信息 
​		https://example.com/api/User/add POST 添加新的用户

​	规范后的RESTful风格的url，形式固定，可读性强，根据users名词和http动词就可以操作这些资源

​		https://example.com/api/users GET 获取所有用户信息 
​		https://example.com/api/users/1 GET 获取标识为1用户信息 
​		https://example.com/api/users/1 DELETE 删除标识为1用户信息 
​		https://example.com/api/users/1 Patch 更新标识为1用户部分信息,包含在body中 
​		https://example.com/api/users POST 添加新的用户

######统一返回数据格式 
​	对于合法的请求应该统一返回数据格式，这里演示的是json

​		1.code——包含一个整数类型的HTTP响应状态码。

​		2.status——包含文本：”success”，”fail”或”error”。HTTP状态响应码在500-599之间为”fail”，在400-499之间为”error”，其它均为”success”（例如：响应状态码为1XX、2XX和3XX）。这个根	据实际情况其实是可要可不要的。

​		3.message——当状态值为”fail”和”error”时有效，用于显示错误信息。参照国际化（il8n）标准，它可以包含信息号或者编码，可以只包含其中一个，或者同时包含并用分隔符隔开。

​		4.data——包含响应的body。当状态值为”fail”或”error”时，data仅包含错误原因或异常名称、或者null也是可以的

#####http状态码 
​	在之前开发的客户端的时候，patch、delete、post操作时body响应里面没有任何信息，仅仅只有http status code。HTTP状态码本身就有足够的含义，根据http status code就可以知道删除、添加、修改等是否成功。服务段向用户返回这些状态码并不是一个强制性的约束。简单点说你可以指定这些状态，但是不是强制的。常用HTTP状态码对照表 
HTTP状态码也是有规律的 
​	1**请求未成功 
​	2**请求成功、表示成功处理了请求的状态代码。 
​	3**请求被重定向、表示要完成请求，需要进一步操作。 通常，这些状态代码用来重定向。 
​	4** 请求错误这些状态代码表示请求可能出错，妨碍了服务器的处理。 
​	5\**（服务器错误）这些状态代码表示服务器在尝试处理请求时发生内部错误。 这些错误可能是服务器本身的错误，而不是请求出错。

​     ps: 状态码我就不罗列了，自己去百度，好困

#####合理使用query parameter 
​	在请求数据时，客户端经常会对数据进行过滤和分页等要求，而这些参数推荐采用HTTP Query Parameter的方式实现

比如设计一个最近登陆的所有用户

​		https://example.com/api/users?recently_login_day=3

​	搜索用户，并按照注册时间降序

​		https://example.com/api/users?q=key&sort=create_title_desc

​	搜索用户，并按照注册时间升序、活跃度降序

​		https://example.com/api/users?q=key&sort=create_title_asc,liveness_desc

​	关于分页，看看博客园开放平台分页获取精华区博文列表

​		https://api.cnblogs.com/api/blogposts/@picked?pageIndex={pageIndex}&pageSize={pageSize} 
返回示例： 

~~~json
[ 
	{ 
        “Id”: 1, 
        “Title”: “sample string 2”, 
        “Url”: “sample string 3”, 
        “Description”: “sample string 4”, 
        “Author”: “sample string 5”, 
        “BlogApp”: “sample string 6”, 
        “Avatar”: “sample string 7”, 
        “PostDate”: “2017-06-25T20:13:38.892135+08:00”, 
        “ViewCount”: 9, 
        “CommentCount”: 10, 
        “DiggCount”: 11 
	}, 
    { 
        “Id”: 1, 
        “Title”: “sample string 2”, 
        “Url”: “sample string 3”, 
        “Description”: “sample string 4”, 
        “Author”: “sample string 5”, 
        “BlogApp”: “sample string 6”, 
        “Avatar”: “sample string 7”, 
        “PostDate”: “2017-06-25T20:13:38.892135+08:00”, 
        “ViewCount”: 9, 
        “CommentCount”: 10, 
        “DiggCount”: 11 
	} ，
]
~~~

#####多表、多参数连接查询如何设计URL 
​	这是一个比较头痛的问题，在做单个实体的查询比较容易和规范操作，但是在实际的API并不是这么简单而已，这其中常常会设计到多表连接、多条件筛选、排序等。 
比如我想查询一个获取在6月份的订单中大于500元的且用户地址是北京，用户年龄在22岁到40岁、购买金额降序排列的订单列表

​		https://example.com/api/orders?order_month=6&order_amount_greater=500&address_city=北京&sort=order_amount_desc&age_min=22&age_max=40

​	从这个URL上看，参数众多、调用起来还得一个一个仔细对着，而且API本身非常不容易维护，命名看起来不是很容易，不能太长，也不能太随意。

在.net WebAPI总我们可以使用属性路由，属性路由就是讲路由附加到特定的控制器或操作方法上装饰Controll及其使用[Route]属性定义路由的方法称为属性路由。 
这种好处就是可以精准地控制URL，而不是基于约定的路由，简直就是为这种多表查询量身定制似的的。 
从webapi 2开发，现在是RESTful API开发中最推荐的路由类型 
我们可以在Controll中标记Route 
	**[Route(“api/orders/{address}/{month}”)] **
	Action中的查询参数就只有金额、排序、年龄。减少了查询参数、API的可读性和可维护行增强了。

​		https://example.com/api/orders/beijing/6?order_amount_greater=500&sort=order_amount_desc&age_min=22&age_max=40

​	这种属性路由比如在博客园开放的API也有这方面的应用，如获取个人博客随笔列表

​	请求方式：GET 
​	请求地址：https://api.cnblogs.com/api/blogs/{blogApp}/posts?pageIndex={pageIndex} 
​	blogApp：博客名

#####API请求授权 
​	博客园的开放平台API也是基于OAuth2.0，参见理解OAuth2.0，留存分析


 

