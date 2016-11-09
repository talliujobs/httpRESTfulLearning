

# RESTful介绍 #
为什么要用RESTful, 知乎毛草网友说:
>我倒不是说一定要把所有API按照RESTful的方式设计.

>但是, 第一这个东西现在流行;

>第二你设计接口的时候有规范太重要了.

>第三, 等你需要优化的时候再去改某些细节的东西也不迟. 就算你不用RESTful, 那也要制定一套完整的规范才行. 否则以后会死的.

>这也是RPC最大的问题. 自由是自由, 可是这种自由是以未来维护的复杂度作为代价的. 都不用多,你的系统运行个半年, 再维护的时候就已经会死人了好不好.


当然RESTful的优点不仅仅在于此, 大致说来有如下的优点:
* 规范. 统一的架构风格, 使得开发 测试 运维工作变得统一. 良好的符合”RESTful风格”的URI设计,可以让Web接口的功能和整体结构更加清晰, 仅仅通过URI就能方便的推测出来接口是做什么的, 以及多个资源之间关联性同时带来更好的兼容性.
* 语义. 充分利用 HTTP 协议本身语义, 对资源的操作正好对应相应的HTTP动词.
* 安全. 由于采用的是HTTP动词的操作, 请求所造成的影响明确, 或者说副作用明确,比如GET肯定是安全的, PUT和DELETE肯定是幂等的.
* 无状态. 在调用一个接口（访问、操作资源）的时候, 可以不用考虑上下文, 不用考虑当前状态, 极大的降低了复杂度

## RESTful定义 ##
Wikipedia的解释是这样
>表述性状态转移（英文：Representational State Transfer, 简称REST）是Roy Fielding博士于2000年在他的博士论文中提出来的一种软件架构风格.

>Roy Fielding是 HTTP 规范的主要编写者之一. 他在论文中提到: “我这篇文章的写作目的, 就是想在符合架构原理的前提下, 理解和评估以网络为基础的应用软件的架构设计, 得到一个功能强、性能好、适宜通信的架构. REST指的是一组架构约束条件和原则.” 如果一个架构符合REST的约束条件和原则, 我们就称它为RESTful架构.

>REST本身并没有创造新的技术、组件或服务, 而隐藏在RESTful背后的理念就是使用Web的现有特征和能力, 更好地使用现有Web标准中的一些准则和约束. 虽然REST本身受Web技术的影响很深, 但是理论上REST架构风格并不是绑定在HTTP上, 只不过目前HTTP是唯一与REST相关的实例. 所以我们这里描述的REST也是通过HTTP实现的REST.

全称其实是Resource Representational State Transfer, 即:资源在网络中以某种表现形式进行状态变化.

## 资源与URI(Resource AND URI) ##

### 资源的定义 ###

任何事物, 只要有被引用到的必要, 它就是一个资源. 资源可以是实体(例如手机号码), 也可以只是一个抽象概念(例如价值). 从消费者的观点看, 资源可以是消费者能够与之交互已达成目标的任何东西. 比如买书的发票, 订单, 用户的个人信息等等.

### 标识符 ###

对一个资源的访问, 总是通过资源的表述方式来间接的完成的. 资源要被使用, 必须能够在网络上标识它, 同时还需要有某些手段来操作资源. 在WEB中我们使用URI来唯一标识一个资源, 使得该资源在WEB上可寻址, 且能够使用协议来操作. 一个资源的URI将它与任何其它资源分开, 可以通过这个URI来与其它资源交互.

一个典型的RESTful URI可能是这样:

>https://www.jiabangou.com/v0.1/{resource}/{resource-id}/{sub-resource}/{sub-resource-id}/{sub-resource-property}


以spring boot为例, github是这样设计的:
```
https://github.com/spring-projects
https://github.com/spring-projects/spring-boot
https://github.com/spring-projects/spring-boot/issues
https://github.com/spring-projects/spring-boot/issues?q=is%3Aclosed
```
URI的设计要遵循可读性, 可寻址的一些原则.
* URI代表资源的路径(位置), 以及一些特殊的操作
* 使用名词, 复数进行资源命名, 不管资源返回单个还是多个
* 单词使用小写字母, 数字, 多个单词间使用中划线(-)或下划线(_)
* 资源路径从父路径到子路径在到下一级路径, 所有资源放到一域名下
* 在资源路径之前, 使用版本号. 也有的是将版本号放到Header中, 如github.
* 使用?来进行来进行资源的条件过滤, 如分页
* 使用, 或 :来表示同级资源关系
* 优先使用内容协商来区分表述格式, 而不是使用后缀来区分表述格式
* 建议使用SSL

关于资源和URI的关系, 一句话总结是: URI唯一标识网络上的一个资源, 资源的消费者和资源的发布方通过内容协商确定返回响应的表述形式.   
考虑下面的URI
```
GET: http://www.jiabangou.com/users/1
```
* users/1 代表用户资源, 该用户ID为1, http://www.jiabangou.com/users/1是URI
* 请求上面的URI会返回资源users/1的表现形式, 表现形式是资源的一组属性的集合, user.name user.mobile user.password
* 资源的表述支持各种格式, 如xml json html, URI可以将资源映射到一种具体的表述形式上. 如: http://www.jiabangou.com/users.json http://www.jiabangou.com/users.xml

对于每一个资源表述是否需要指定格式, 有待大家讨论.「rest实战」的作者认为
>URI对于消费者来说应该是不透明的, 只有URI的发布者才知道如何来解释它和将它应映射到一个资源. 使用类似xml .html 或 .json的扩展名是一个历史性约定, 来自WEB服务器简单地将URI映射到文件的古老时代.


## 统一资源接口(Uniform Interface) ##
RESTful架构遵循统一接口原则, 统一接口包含了一组受限的预定义的操作, 不论什么样的资源, 都是通过使用相同的接口进行资源的访问. 接口应该使用标准的HTTP方法如GET, PUT, PATCH和POST, 并遵循这些方法的语义. 如果按照HTTP方法的语义来暴露资源, 那么接口将会拥有安全性和幂等性的特性, 关于HTTP幂等后面在进行讨论.

简言之: RESTful必须通过统一的接口来对资源执行各种操作, 对于每个资源只能执行一组有限的操作. 以HTTP/1.1协议为例, HTTP/1.1协议定义了一个操作资源的统一接口, 主要包括以下内容, 后文会一一介绍.
* HTTP方法(GET POST PUT DELETE PATCH HEAD OPTIONS)
* HTTP头信息
* HTTP响应信息
* 内容协商机制
* 缓存机制
* 安全认证机制

### 请求方法 ###
|方法名称|是否幂等|是否安全|备注|
|---|---|---|---|
|OPTIONS|是|是|使用该方法来获取资源支持的HTTP方法列表. 这个方法会请求服务器返回该资源所支持的所有HTTP请求方法, 该方法会用’*’来代替资源名称, 向服务器发送OPTIONS请求, 可以测试服务器功能是否正常|
|HEAD|是|是|用于只获取请求某个资源返回的头信息|
|GET|是|是|用于从服务器获取某个资源的信息 <br>完成请求后返回状态码 200 OK <br> 完成请求后需要返回被请求的资源详细信息|
|POST|否|否|用于创建新资源 <br> 创建完成后返回状态码 201 Created <br> 完成请求后需要返回被创建的资源详细信息|
|PUT|是|否|用于完整的替换资源或者创建指定身份的资源, 比如创建id为123的某个资源 <br> 如果是创建了资源, 则返回 201 Created <br> 如果是替换了资源, 则返回 200 OK <br> * 完成请求后需要返回被修改的资源详细信息|
|PATCH|否|否|用于局部更新资源 <br> 完成请求后返回状态码 200 OK <br> 完成请求后需要返回被请求的资源详细信息|
|DELETE|是|否|用于删除某个资源, 有的客户端不支持, 服务器端和客户端相互妥协, 如客户端通过隐藏的参数_method=DELETE来完成请求后返回状态码 204 No Content|



>安全性：任意多次对同一资源操作，都不会导致资源的状态变化  
幂等性：任意次对同一资源操作，对资源的改变是一样的
关于安全性与幂等性，参见我的另一篇文章  [Idempotence.md](./Idempotence.md)
幂等到底有什么用, 先来看几个需求
* 创建订单, 每次请求只能创建一个订单
* 发送短信或邮件, 每次发送只发送一条短信
* 即使第三方系统故障, 或是网络问题, 用户支付只扣一次钱  
在分布式系统及微服务中，因为网络原因而导致调用系统未能获取到确切的结果从而导致重试，是非常常见的情景. 这种情况下, 就需要接口具有幂等性. 比如上面创建订单的问题.

需要注意的是, 有的客户端并不支持所有的HTTP方法, 如PUT或DELETE方法. 解决办法是客户端通过提交一个隐藏的_method=PUT字段来传递真实的请求方法. 而某些JS框架, 通过设置X-HTTP-Method-Override 这个HTTP Header来避免这个问题，见下方例子（ 注意headers部分）
```
/**
* 发起一个PUT请求
* @param url
* @param formData
* @returns Promise
*/
function put(url, jsonData) {
  return $.ajax({
      url: HOST + url,
      type: 'PUT',
      contentType: 'application/json',
      data: JSON.stringify(jsonData),
      cache: false,
      timeout: TIMEOUT,
      headers: {
          'X-HTTP-Method-Override': 'PUT',
          'X-Xsrftoken': Cookie.get('_xsrf')
      }
  }).then(responseFilter);
}
```
### 状态码 ###
使用HTTP动词对资源进行操作, 常见的HTTP状态码含义如下. 关于HTTP状态码及含义请参考wikipedia上的详细介绍
#### 请求成功 2XX ####
* 200 OK : 请求执行成功并返回相应数据, 如 GET 成功
* 201 Created : 对象创建成功并返回相应资源数据, 如 POST 成功; 创建完成后响应头中应该携带头标 Location, 指向新建资源的地址
* 202 Accepted : 接受请求, 但无法立即完成创建行为, 比如其中涉及到一个需要花费若干小时才能完成的任务. 返回的实体中应该包含当前状态的信息, 以及指向处理状态监视器或状态预测的指针, 以便客户端能够获取最新状态.
* 204 No Content : 请求执行成功, 不返回相应资源数据, 如 PATCH DELETE 成功

#### 重定向 3XX ####
重定向的新地址都需要在响应头 Location 中返回
* 301 Moved Permanently : 被请求的资源已永久移动到新位置
* 302 Found : 请求的资源现在临时从不同的 URI 响应请求
* 303 See Other : 对应当前请求的响应可以在另一个 URI 上被找到, 客户端应该使用 GET 方法进行请求
* 304 Not Modified : 资源自从上次请求后没有再次发生变化, 主要使用场景在于实现数据缓存
* 307 Temporary Redirect : 对应当前请求的响应可以在另一个 URI 上被找到, 客户端应该保持原有的请求方法进行请求

#### 客户端错误 4XX ####
* 400 Bad Request : 请求体包含语法错误
* 401 Unauthorized : 需要验证用户身份, 如果服务器就算是身份验证后也不允许客户访问资源, 应该响应 403 Forbidden
* 403 Forbidden : 服务器拒绝执行
* 404 Not Found : 找不到目标资源
* 405 Method Not Allowed : 不允许执行目标方法, 响应中应该带有 Allow 头, 内容为对该资源有效的 HTTP 方法
* 406 Not Acceptable : 服务器不支持客户端请求的内容格式, 但响应里会包含服务端能够给出的格式的数据, 并在 Content-Type 中声明格式名称
* 409 Conflict : 请求操作和资源的当前状态存在冲突。主要使用场景在于实现并发控制
* 412 Precondition Failed : 服务器在验证在请求的头字段中给出先决条件时，没能满足其中的一个或多个。主要使用场景在于实现并发控制


#### 服务器端错误 5XX ####
* 501 Internal Server Error : 服务器遇到了一个未曾预料的状况, 导致了它无法完成对请求的处理
* 501 Not Implemented : 服务器不支持当前请求所需要的某个功能
* 502 Bad Gateway : 作为网关或者代理工作的服务器尝试执行请求时, 从上游服务器接收到无效的响应
* 503 Service Unavailable : 由于临时的服务器维护或者过载, 服务器当前无法处理请求. 这个状况是临时的, 并且将在一段时间以后恢复. 如果能够预计延迟时间, 那么响应中可以包含一个 Retry-After 头用以标明这个延迟时间（内容可以为数字, 单位为秒; 或者是一个 HTTP 协议指定的时间格式）. 如果没有给出这个 Retry-After 信息, 那么客户端应当以处理 500 响应的方式处理它.


**统一资源接口要求使用标准的HTTP方法对资源进行操作, 所以URI只应该来表示资源的名称, 而不应该包括资源的操作. 即: URI不应该使用动作来描述.**


## 资源的表述(Representation) ##
资源的表述是一段对于资源在某个特定时刻的状态的描述, 可以在客户端-服务器端之间转移（交换）. 资源在外界可以有多种格式(表现形式), 同一个文本资源, 可以使用json xml text等格式; 图片资源可以使用png jpg等格式.

客户端如何获取或者说知道服务器端提供什么样的表述形式呢? 答案是HTTP内容协商.

### HTTP内容协商(Content Negotiation) ###
有时候, 同一个 URL 可以提供多份不同的文档, 比如访问google首页, google需要根据语言设置显示对应的语言, 提供不同语言版本的内容. 这就要求服务端和客户端之间有一个选择最合适版本的机制, 这就是内容协商。

更为正式一点的解释是这样: 客户端和服务器端就响应的资源内容进行交涉, 然后服务器端提供给客户端最为适合的资源.

HTTP 的内容协商通常的方案是: 服务端根据客户端发送的请求头中某些字段自动发送最合适的版本. 可以用于这个机制的请求头字段又分两种：内容协商专用字段（Accept 字段）、其他字段。

HTTP中关于 Accept 字段定义

|HTTP请求头字段|请求说明|响应头字段|
|---|---|---|
|Accept|告知服务器发送何种媒体类型|Content-Type|
|Accept-Language|告知服务器发送何种语言|Content-Language|
|Accept-Charset|告知服务器发送何种字符集|Content-Type|
|Accept-Encoding|告知服务器采用何种压缩方式|Content-Encoding|

**服务端和客户端协商好返回资源的表现形式, 客户端通过HTTP请求头的Accept头请求特定的格式, 服务器端通过Content-Type告诉客户端资源的表述形式. **  

## 状态的转移 (State Transfer) ##

HTTP协议是一种无状态的协议, 每次的请求都是独立的, 它的执行情况和结果与前面的请求和之后的请求是无直接关系的, 它不会受前面的请求应答情况直接影响, 也不会直接影响后面的请求应答情况.

客户端与服务端的交互必须是无状态的, 并在每一次请求中包含处理该请求所需的一切信息. 服务端不需要在请求间保留应用状态, 只有在接受到实际请求的时候, 服务端才会关注应用状态. 这种无状态通信原则, 使得服务端和中介能够理解独立的请求和响应. 在多次请求中, 同一客户端也不再需要依赖于同一服务器, 方便实现高可扩展和高可用性的服务端.

RESTful的状态转移是指在客户端和服务器端之间转移（transfer）代表资源状态的表述. 通过转移和操作资源的表述, 来间接实现操作资源的目的.



## Laravel RESTful ##

RESTful的跳转规则

|请求方式|请求URL|对应控制器的方法|用于哪方面|备注|
|------|:---:|:---:|
|GET|/test|index() |页面展示/列表展示||
|GET|/test/create |create() |添加/新建 页面的展示||
|POST|/test|store() |存储新添加的数据||
|GET|/test/{id}/edit  |edit($id)|修改页面||
|PUT|/test/{id} |update($id) |接收修改的方法| {{ method_field('PUT') }}  等价于 ``` <input type="hidden" name="_method" value="put"/> ```  |
|GET|/test/{id}|destroy($id)|删除|{{ method_field('DELETE') }}  等价于 ``` <input type="hidden" name="_method" value="delete"/> ```  |
