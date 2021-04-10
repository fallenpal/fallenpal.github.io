# CORS(跨域资源共享）笔记




## CORS（跨域资源共享）是什么?
<!--more-->

CORS，全称是"跨域资源共享"（Cross-origin resource sharing），是浏览器上控制跨域名通信的一种机制。
<!--more-->

更准确地说，跨域是一种访问控制机制，Web服务器基于HTTP Header的信息，决定其他Origin（Origin由域名、协议和端口组成）是否可以访问其资源，并通知浏览器进行允许或阻断。
An **HTTP-header ** based mechanism that allows a server to indicate any other origins (**domain, protocol, or port**) than its own from which a browser should permit loading of resources. 

定义里的几个关键字：首先是通过HTTP Header进行控制的。然后可以基于协议、域名和端口。

跨域请求的一个例子：运行在http://a.com 的JS代码，使用[[XHR]]发起一个到http://b.com/data.json的请求


CORS以来Not implemented on e.g. cURL, WGET etc.

### CORS有哪些场景
详细的范围在[MDN的说明](https://developer.mozilla.org/en-US/docs/Web/HTTP/CORS#what_requests_use_cors)，一些典型的CORS场景有：foo.com需要从 bar.com获取资源，方式为：
- 使用XHR或者Fetch API调用
- 使用CSS内 `@font-face` 获取字体文件

需要注意的是，如果只是HTML内用`<img>`的方式获取另一个域的文件，这并不算是跨域请求。但是存放图片的域名往往依然会返回允许跨域的Header，这是因为要考虑到有很多通过XHR获取图片的场景。

### 为什么需要CORS？

跨域的主要作用，是让服务器了解，访问都是从哪些其他域名过来的。另外可以基于来访的域名，进行一定程度的控制。

相比`Referer` header，跨域更难伪装，因为跨域是在浏览器层面做的安全检查。浏览器会指定自己发送的`Origin` header，而我们无法从浏览器内覆盖`Origin`header。

然而，使用curl，Python等方式可以自己构建访问，并自由指定`Origin`, 所以不能完全指望CORS进行严格的安全校验。CORS的更大意义是让服务器知道这个跨域访问是从什么站点发起的。如果需要鉴权，还是应当使用cookie或者其他token鉴权办法。

### 什么情况算跨域？

一个“域”由协议、域名和端口组成，因此以下这些域名，在浏览器看来，属于不同的域，如果有彼此之间的互访。即被认为跨域访问：
- http://www.foo.com
- https://www.foo.com
- http://api.foo.com
- http://www.foo.com:88
- http://www.bar.com




## CORS是如何工作的

CORS需要浏览器和服务器同时支持。目前所有浏览器都支持，但如果客户端使用Curl等方式，是可以绕开检查，不做CORS的。

CORS机制通过让服务器在响应中增加一系列的header，明确哪些源站允许读取它的信息。如果出现任何错误，可以在[[Chrome Console]]内查看到有关错误代码。

### CORS的分类

CORS按照复杂程度分为两类：
1. 简单请求
同时满足以下条件的，就是简单请求：
- 使用这些Method之一：GET, HEAD, POST
- HTTP Header没有超出以下几种：
	- Accept
	- Accept-Language
	- Content-Language
	- Last-Event-ID
	- Content-Type：只限于三个值application/x-www-form-urlencoded、multipart/form-data、text/plain


2. 复杂请求

不属于简单请求的，就是复杂请求。

### 简单请求

客户端在访问`www.foo.com`的网页，返回的HTML里有JS代码让客户端发起到`www.bar.com`的某个资源的请求。客户端向`www.bar.com`发起Get请求, 浏览器会自动识别到这个请求是去往不同的domain。于是浏览器自动添加一个 `Origin` header （这个header无法被用户手动修改，因此比referer具有更好的安全性）：

```
Origin: https://www.foo.com
```

服务器对`Origin`进行检查，如果在服务器的允许范围内，则Response Header中，包含以下字段：

```
Access-Control-Allow-Origin: <value-of-Origin-header> | *
```

`*` 代表允许所有域名，可以简化服务器的配置，但会降低安全性。并且在某些CORS场景中，如果响应的ACAO值为`*`的话，则无法继续进行（见credentials章节介绍）。

需要注意的是，ACAO的value不可以是多个域名被空格或逗号分开等形式，只可以是一个域名。如果服务器希望同时允许多个Origin，可以在服务器上通过程序实现对应逻辑。例如设置一个Origin 列表，并检查访问的`Origin: {hostname}` 是否在列表内，如果是的话， 就返回对应的 `Access-Control-Allow-Origin: {hostname}`

下图为详细的CORS简单请求的流程图:

![](http://image-fallenpal.test.upcdn.net/Pasted%20image%2020201118215250.png)

#### 包含credentials的请求

默认情况下，在XHR和Fetch的场景里，客户端都不会发送任何Credentials。如果客户端发送的请求中指定了会包含credential（`withCredentials` 为`True`)：
```javascript
const invocation = new XMLHttpRequest();
const url = 'http://bar.other/resources/credentialed-content/';
    
function callOtherDomain() {
  if (invocation) {
    invocation.open('GET', url, true);
    invocation.withCredentials = true;
    invocation.onreadystatechange = handler;
    invocation.send(); 
  }
}
```

那么服务器的响应必须包含`Access-Control-Allow-Credentials: true `，否则这个响应会被客户端忽略。另外当credential为true的时候，access-control-allow-origin也不能把再设置为*，因为不被认为是安全的，所以会被拒绝。

解决办法：
1. 不再使用credentials
2. 把acao的value修改为明细的域名
### 复杂请求

当请求更加“复杂”的时候，例如HTTP method不是GET，POST或HEAD，以及content-type特殊，或者请求的header特殊（这些特殊字段都可以查询MDN规范得到，不必记忆）。

客户端会先发送一个method为OPTION的preflight 请求，example如下：

```
OPTIONS /doc HTTP/1.1
Host: bar.other
User-Agent: Mozilla/5.0 (Macintosh; Intel Mac OS X 10.14; rv:71.0) Gecko/20100101 Firefox/71.0
Accept: text/html,application/xhtml+xml,application/xml;q=0.9,*/*;q=0.8
Accept-Language: en-us,en;q=0.5
Accept-Encoding: gzip,deflate
Connection: keep-alive
Origin: http://foo.example

//以下两个header明确了需要使用的method和特殊的header
Access-Control-Request-Method: POST
Access-Control-Request-Headers: X-PINGOTHER, Content-Type
```

服务器检查请求的Method和Header后，如果同意，则发送对应的响应，表明同意后续的CORS请求。之后客户端才会发送正式的请求：
```
HTTP/1.1 204 No Content
Date: Mon, 01 Dec 2008 01:15:39 GMT
Server: Apache/2
Access-Control-Allow-Origin: https://foo.example
Access-Control-Allow-Methods: POST, GET, OPTIONS
Access-Control-Allow-Headers: X-PINGOTHER, Content-Type
Access-Control-Max-Age: 86400
Vary: Accept-Encoding, Origin
Keep-Alive: timeout=2, max=100
Connection: Keep-Alive
```

响应中，`Access-Control-Allow-Methods`表明了服务器允许POST等method，`Access-Control-Allow-Headers`表明了服务器允许了`X-PINGOTHER`等特殊header。因此浏览器获取了服务器的允许，会进行后续的跨域请求。

后续的CORS请求和简单请求完全相同。

由于preflight 带来了一次额外的访问，增加了延迟和服务器处理的overhead，所以服务器响应中包含了`Access-Control-Max-Age`, 作用为让浏览器缓存这个信息，后续的访问不再进行preflight的确认。

以下为详细流程图：

![](http://image-fallenpal.test.upcdn.net/Pasted%20image%2020201118215652.png)


