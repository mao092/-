# python接口自动化测试（一）
 本节开始，开始介绍python的接口自动化测试，首先需要搭建python开发环境，到https://www.python.org/下载python

版本直接安装就以了，建议 下载python2.7.11版本，当然，也是可以下载python最新版本的。

       接口测试是测试系统组件间接口的一种测试。接口测试主要用于检测外部系统与系统之间以及内部各个子系统之间的交互点。

测试的重点是要检查数据的交换，传递和控制管理过程，以及系统间的相互逻辑依赖关系等，该解释来自百度百科。

       当然，为了更好的进行接口测试，需要了解经常使用的http状态消息，比如请求成功是200 OK，但是http状态消息除了这个之

外还有很多的，http的状态消息，简单的来理解就是当浏览器从web服务器发送请求时，可能会请求成功，可能请求失败返回其他的错

误信息，从而返回各种情况的htttp状态消息。比如百度首页输入搜索关键词，可能会返回成功的后的搜索信息，但是也有可能搜索失败

的情况，当然这种情况一般很少出现，毕竟百度不会出现这么低级的错误。下面分别列出经常常见的http状态消息，这些信息来自w3c

网站，见如下的http状态消息:

    1xx: 信息
消息:	描述:
100 Continue	服务器仅接收到部分请求，但是一旦服务器并没有拒绝该请求，客户端应该继续发送其余的请求。
101 Switching Protocols	服务器转换协议：服务器将遵从客户的请求转换到另外一种协议。
    2xx: 成功
消息:	描述:
200 OK	请求成功（其后是对GET和POST请求的应答文档。）
201 Created	请求被创建完成，同时新的资源被创建。
202 Accepted	供处理的请求已被接受，但是处理未完成。
203 Non-authoritative Information	文档已经正常地返回，但一些应答头可能不正确，因为使用的是文档的拷贝。
204 No Content	没有新文档。浏览器应该继续显示原来的文档。如果用户定期地刷新页面，而Servlet可以确定用户文档足够新，这个状态代码是很有用的。
205 Reset Content	没有新文档。但浏览器应该重置它所显示的内容。用来强制浏览器清除表单输入内容。
206 Partial Content	客户发送了一个带有Range头的GET请求，服务器完成了它。
    3xx: 重定向
消息:	描述:
300 Multiple Choices	多重选择。链接列表。用户可以选择某链接到达目的地。最多允许五个地址。
301 Moved Permanently	所请求的页面已经转移至新的url。
302 Found	所请求的页面已经临时转移至新的url。
303 See Other	所请求的页面可在别的url下被找到。
304 Not Modified	未按预期修改文档。客户端有缓冲的文档并发出了一个条件性的请求（一般是提供If-Modified-Since头表示客户只想比指定日期更新的文档）。服务器告诉客户，原来缓冲的文档还可以继续使用。
305 Use Proxy	客户请求的文档应该通过Location头所指明的代理服务器提取。
306 Unused	此代码被用于前一版本。目前已不再使用，但是代码依然被保留。
307 Temporary Redirect	被请求的页面已经临时移至新的url。
    4xx: 客户端错误
消息:	描述:
400 Bad Request	服务器未能理解请求。
401 Unauthorized	被请求的页面需要用户名和密码。
402 Payment Required	此代码尚无法使用。
403 Forbidden	对被请求页面的访问被禁止。
404 Not Found	服务器无法找到被请求的页面。
405 Method Not Allowed	请求中指定的方法不被允许。
406 Not Acceptable	服务器生成的响应无法被客户端所接受。
407 Proxy Authentication Required	用户必须首先使用代理服务器进行验证，这样请求才会被处理。
408 Request Timeout	请求超出了服务器的等待时间。
409 Conflict	由于冲突，请求无法被完成。
410 Gone	被请求的页面不可用。
411 Length Required	"Content-Length" 未被定义。如果无此内容，服务器不会接受请求。
412 Precondition Failed	请求中的前提条件被服务器评估为失败。
413 Request Entity Too Large	由于所请求的实体的太大，服务器不会接受请求。
414 Request-url Too Long	由于url太长，服务器不会接受请求。当post请求被转换为带有很长的查询信息的get请求时，就会发生这种情况。
415 Unsupported Media Type	由于媒介类型不被支持，服务器不会接受请求。
416 	服务器不能满足客户在请求中指定的Range头。
417 Expectation Failed	 
    5xx: 服务器错误
消息:	描述:
500 Internal Server Error	请求未完成。服务器遇到不可预知的情况。
501 Not Implemented	请求未完成。服务器不支持所请求的功能。
502 Bad Gateway	请求未完成。服务器从上游服务器收到一个无效的响应。
503 Service Unavailable	请求未完成。服务器临时过载或当机。
504 Gateway Timeout	网关超时。
505 HTTP Version Not Supported	服务器不支持请求中指明的HTTP协议版本。
 

     对于接口测试来说，一般分为二种情况，分别是基于http协议和基于web services协议，但是最常用的是基于http协议的

接口测试，其中最常用的http方法是get和post，当然还有put,delete请求，接口测试的过程就是client(浏览器)向server(服务

器端)request一个请求,server得到请求后,response返回给client响应数据。下面分别说明接口测试中几种常使用的请求方法：

   GET:从指定的资源获取数据

如在百度阅读搜索“selenium-python自动化测试“，就会返回本人写的《selenium-python自动化测试》电子书，请求地址

为：http://yuedu.baidu.com/search?word=selenium-python%E8%87%AA%E5%8A%A8%E5%8C%96%E6%

B5%8B%E8%AF%95,方式为GET，见请求后返回的结果：



 

   POST:向指定的资源要被处理的数据

对于post请求，以百度登录为案例，来说明这一过程，请求地址为：http://www.cyw.com/api/login/authorized.html，

请求方式为POST，见如下的截图：

  

 

   PUT:上传指定的URL，一般是修改，可以理解为数据库中的update。

   DELETE：删除指定资源。

   在接口测试中，一般来说,post创建数据，get获取创建成功后的所有数据和指定的数据,put可以对创建成功后的数据

进行修改，delete是指定的资源。

   当然，接口自动化相比UI自动化来说，比较复杂，需要掌握的知识比较多，本人也是在学习中，感觉接口自动化测试，

首先需要了解http状态消息，http协议，http方法,当然还得了解python语言，毕竟接口自动化测试是以代码的方式进行，

并非工具的方式。
