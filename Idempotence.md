
*幂等性定义*

在HTTP/1.1规范中幂等性的定义是：
>Methods can also have the property of "idempotence" in that (aside from error or expiration issues) the side-effects of N > 0 identical requests is the same as for a single request.

从定义上看，HTTP方法的幂等性是指一次和多次请求某一个资源应该具有同样的副作用。幂等性属于语义范畴，正如编译器只能帮助检查语法错误一样，HTTP规范也没有办法通过消息格式等语法手段来定义它，这可能是它不太受到重视的原因之一。但实际上，幂等性是分布式系统设计中十分重要的概念，而HTTP的分布式本质也决定了它在HTTP中具有重要地位。

下面将介绍HTTP GET、DELETE、PUT、POST四种主要方法的语义和幂等性。

HTTP GET方法用于获取资源，不应有副作用，所以是幂等的。比如：GET http://www.bank.com/account/123456，不会改变资源的状态，不论调用一次还是N次都没有副作用。请注意，这里强调的是一次和N次具有相同的副作用，而不是每次GET的结果相同。GET http://www.news.com/latest-news这个HTTP请求可能会每次得到不同的结果，但它本身并没有产生任何副作用，因而是满足幂等性的。

HTTP DELETE方法用于删除资源，有副作用，但它应该满足幂等性。比如：DELETE http://www.forum.com/article/4231，调用一次和N次对系统产生的副作用是相同的，即删掉id为4231的帖子；因此，调用者可以多次调用或刷新页面而不必担心引起错误。

比较容易混淆的是HTTP POST和PUT。POST和PUT的区别容易被简单地误认为“POST表示创建资源，PUT表示更新资源”；而实际上，二者均可用于创建资源，更为本质的差别是在幂等性方面。在HTTP规范中对POST和PUT是这样定义的：

>The POST method is used to request that the origin server accept the entity enclosed in the request as a new subordinate of the resource identified by the Request-URI in the Request-Line ...... If a resource has been created on the origin server, the response SHOULD be 201 (Created) and contain an entity which describes the status of the request and refers to the new resource, and a Location header.

>The PUT method requests that the enclosed entity be stored under the supplied Request-URI. If the Request-URI refers to an already existing resource, the enclosed entity SHOULD be considered as a modified version of the one residing on the origin server. If the Request-URI does not point to an existing resource, and that URI is capable of being defined as a new resource by the requesting user agent, the origin server can create the resource with that URI.

POST所对应的URI并非创建的资源本身，而是资源的接收者。比如：POST http://www.forum.com/articles的语义是在http://www.forum.com/articles下创建一篇帖子，HTTP响应中应包含帖子的创建状态以及帖子的URI。两次相同的POST请求会在服务器端创建两份资源，它们具有不同的URI；所以，POST方法不具备幂等性。而PUT所对应的URI是要创建或更新的资源本身。比如：PUT http://www.forum/articles/4231的语义是创建或更新ID为4231的帖子。对同一URI进行多次PUT的副作用和一次PUT是相同的；因此，PUT方法具有幂等性。

在介绍了几种操作的语义和幂等性之后，我们来看看如何通过Web API的形式实现前面所提到的取款功能。很简单，用POST /tickets来实现create_ticket；用PUT /accounts/account_id/ticket_id&amount=xxx来实现idempotent_withdraw。值得注意的是严格来讲amount参数不应该作为URI的一部分，真正的URI应该是/accounts/account_id/ticket_id，而amount应该放在请求的body中。这种模式可以应用于很多场合，比如：论坛网站中防止意外的重复发帖。



上面反复提到幂等, 那么什么是幂等, REST中的幂等又是个什么概念?

幂等的数学定义是:

    对于单目运算, 如果一个运算对于在范围内的所有的一个数多次进行该运算所得的结果和进行一次该运算所得的结果是一样的, 那么我们就称该运算是幂等的.

比如求绝对值, 在实数集中, 有abs(a)=abs(abs(a)). 具体点说, 比如求 -10 的绝对值, 求一次绝对值和求100次绝对值, 结果是相同的, 都是返回10.

    对于双目运算, 则要求当参与运算的两个值是等值的情况下, 如果满足运算结果与参与运算的两个值相等，则称该运算幂等.

如求两个数的最大值max(x, y)=x. 比如 x=y=100, 无论求多少次最大值, 返回结果都是相同的.

幂等性并不属于特定的协议, 它是分布式系统的一种特性; 不论是SOA还是RESTful的Web API设计都应该考虑幂等性.

    GET方法幂等, 无副作用

    GET方法用于获取资源, 不应该有副作用, 所以是幂等的. 比如 http://www.jiabangou.com/v1.0/users/10 这个方法获取用户id为10的信息, 只是读取用户信息, 不会改变资源状态, 不论调用多少次都无副作用. 请注意: 这里强调的是一次和N次具有相同的副作用, 而不是每次GET的结果相同. 比如获取最新新闻http://www.jiabangou.com/latest-news, 每次请求结果都会不同, 但他本身无任何副作用, 因此是幂等的.

    一个有意思的讨论是, 如果一个GET操作里面包含了记录日志等操作, 算不算幂等?

    DELETE幂等, 有副作用

    DELETE方法也是幂等的, 调用删除一个id为100的方法, 调用100次和调用1次的副作用是相同的, 就是删除这个用户

    POST/PUT

    POST和PUT方法通常被认为是「POST表示创建资源， PUT表示更新资源」

    RFC规范RFC 2616, Hypertext Transfer Protocol – HTTP/1.1, Method Definitions 对POST和PUT的定义如下

    The POST method is used to request that the origin server accept the entity enclosed in the request as a new subordinate of the resource identified by the Request-URI in the Request-Line.

    The PUT method requests that the enclosed entity be stored under the supplied Request-URI. If the Request-URI refers to an already existing resource, the enclosed entity SHOULD be considered as a modified version of the one residing on the origin server. If the Request-URI does not point to an existing resource, and that URI is capable of being defined as a new resource by the requesting user agent, the origin server can create the resource with that URI.

POST的关键点是, as a new subordinate of the resource identified by the Request-URI in the Request-Line, 即创建一个新的资源;

PUT方法强调, 如果请求的URI指向一个已有的资源, 表示更新; 如果指向的URI不存在,表示新建资源;
举一个更加通俗的例子, 用户请修改profile的用户昵称, 将其修改为X, 一旦更新成功, 后续无论请求同样更新接口多少次, 不会对结果产生影响. 而恰恰相反, 一个POST方法每次都会创建一个新的资源.

关于rest的幂等, 有很多很有意思的讨论
