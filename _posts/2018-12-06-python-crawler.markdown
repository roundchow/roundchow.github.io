---
title:  "【数据分析】Python和爬虫"
subtitle: "基于 Python 实现微信公众号爬虫"
author: "wu"
avatar: "img/authors/tigerCat.jpg"
image: "img/timg16.jpg"
date:   2018-12-06 03:00:00
---

基于 Python 实现微信公众号爬虫

如果对本篇涵盖的产品策划、技术方案、详细代码感兴趣，可以加微信（微信号：superloiwu，加好友请备注：crawler），共同探讨。

说明：

Python版本是3.6

参考：

[《Web crawler》](https://en.wikipedia.org/wiki/Web_crawler)
[《Requests》](http://docs.python-requests.org/en/master/)

----- ----- ----- -----

## 第一篇-微信公众号爬虫的基本原理

手段和目标：通过手机客户端利用 Python 爬微信公众号文章的教程，并对公众号文章做数据分析，为更好的运营公众号提供决策。

### 1-爬虫的基本原理

“爬虫”是一个自动化数据采集工具，你只要告诉它要采集哪些数据，给它一个 URL，就能自动地抓取数据了。其背后的基本原理就是爬虫程序向目标服务器发起 HTTP 请求，接着目标服务器返回响应结果，最后爬虫客户端收到响应并从中提取数据，再进行数据清洗、数据存储工作。

### 2-爬虫的基本流程

爬虫流程也是一个 HTTP 请求的过程。以浏览器访问一个网址为例，从用户输入 URL 开始，客户端通过 DNS 解析查询到目标服务器的 IP 地址，接着与之建立 TCP 连接；连接成功后，浏览器构造一个 HTTP 请求发送给服务器；服务器收到请求之后，从数据库查到相应的数据并封装成一个 HTTP 响应，然后将响应结果返回给浏览器；浏览器对响应内容进行数据解析、提取、渲染并最终展示在你面前。

<div class="scale"><img src="img/resources/crawler/httpflow.png"  alt="httpflow" /></div>

HTTP 协议的请求和响应都必须遵循固定的格式，只有遵循统一的 HTTP 请求格式，服务器才能正确解析不同客户端发的请求，同样地，服务器遵循统一的响应格式，客户端才得以正确解析不同网站发过来的响应。

### 3-HTTP 请求格式

HTTP 请求由请求行、请求头、空行、请求体组成。

<div class="scale"><img src="img/resources/crawler/httprequest.png"  alt="httprequest" /></div>

请求行由三部分组成：

1、第一部分是请求方法，常见的请求方法有 GET、POST、PUT、DELETE、HEAD
2、第二部分是客户端要获取的资源路径
3、第三部分是客户端使用的 HTTP 协议版本号

请求头是客户端向服务器发送请求的补充说明，比如 User-Agent 向服务器说明客户端的身份。

请求体是客户端向服务器提交的数据，比如用户登录时需要提高的账号密码信息。请求头与请求体之间用空行隔开。请求体并不是所有的请求都有的，比如一般的GET都不会带有请求体。

### 4-HTTP 响应格式

HTTP 响应格式与请求的格式很相似，也是由响应行、响应头、空行、响应体组成。

<div class="scale"><img src="img/resources/crawler/httpresponse.png"  alt="httpresponse" /></div>

响应行也包含三部分，分别是服务端的 HTTP 版本号、响应状态码、状态说明。

响应状态码常见有 200、400、404、500、502、304 等等，一般以 2 开头的表示服务器正常响应了客户端请求，4 开头表示客户端的请求有问题，5 开头表示服务器出错了，没法正确处理客户端请求。状态码说明就是对该状态码的一个简短描述。

第二部分就是响应头，响应头与请求头对应，是服务器对该响应的一些附加说明，比如响应内容的格式是什么，响应内容的长度有多少、什么时间返回给客户端的、甚至还有一些 Cookie 信息也会放在响应头里面。

第三部分是响应体，它才是真正的响应数据，这些数据其实就是网页的 HTML 源代码。

### 5-小结

这仅只是一个爬虫基本原理的介绍，涉及的 HTTP 协议的内容非常有限，深入了解HTTP 协议，推荐两本书《图解HTTP》、《HTTP权威指南》。


## 第二篇-使用 Requests 实现一个简单网页爬虫

理解爬虫的基本原理可以帮助我们更好的实现代码。Python 提供了非常多工具去实现 HTTP 请求，但第三方开源库提供的功能更丰富，你无需从 socket 通信开始写，比如使用Pyton内建模块 urllib 请求一个 URL 代码示例如下：

<pre>

import ssl

from urllib.request import Request
from urllib.request import urlopen

context = ssl._create_unverified_context()

# HTTP  请求
request = Request(url="https://foofish.net.pip.html",
                    method="GET",
                    headers={"Host": "foofish.net"},
                    data=None)

# HTTP 相应
response = urlopen(request, context=context)
hraders = response.info() # 响应头
content = response.read() # 响应体
code = response.get() # 状态码

</pre>

发起请求前首先要构建请求对象 Request，指定 url 地址、请求方法、请求头，这里的请求体 data 为空，因为你不需要提交数据给服务器，所以你也可以不指定。urlopen 函数会自动与目标服务器建立连接，发送 HTTP 请求，该函数的返回值是一个响应对象 Response，里面有响应头信息，响应体，状态码之类的属性。

### 1-用Requests实现爬虫

但是，Python 提供的这个内建模块过于低级，需要写很多代码，使用简单爬虫可以考虑 Requests，Requests 在GitHub 有近30k的Star，是一个很Pythonic的框架。先来简单熟悉一下这个框架的使用方式。

#### 安装 requests

<pre>

pip install requests

</pre>

#### GET 请求

<pre>

>>> import requests
>>> r = requests.get("https://httpbin.org/ip")
>>> r
<Response [200]>
>>> r.status_code
200
>>> r.content
b'{\n  "origin": "183.6.129.98"\n}\n'

</pre>

#### POST 请求

<pre>

>>> r = requests.post('http://httpbin.org/post', data = {'key':'value'})

</pre>

#### 自定义请求头

这个经常会用到，服务器反爬虫机制会判断客户端请求头中的User-Agent是否来源于真实浏览器，所以，我们使用Requests经常会指定UA伪装成浏览器发起请求

<pre>

>>> url = 'https://httpbin.org/headers'
>>> headers = {'user-agent': 'Mozilla/5.0'}
>>> r = requests.get(url, headers=headers)

</pre>

#### 参数传递

很多时候URL后面会有一串很长的参数，为了提高可读性，requests 支持将参数抽离出来作为方法的参数（params）传递过去，而无需附在 URL 后面，例如请求 url bin.org/get?key=val ，可使用

<pre>

>>> url = "http://httpbin.org/get"
>>> r = requests.get(url, params={"key":"val"})
>>> r.url
'http://httpbin.org/get?key=val'

</pre>

#### 指定Cookie

Cookie 是web浏览器登录网站的凭证，虽然 Cookie 也是请求头的一部分，我们可以从请求头中将其剥离出来，使用 Cookie 参数指定

<pre>

>>> s = requests.get('http://httpbin.org/cookies', cookies={'from=my': 'browser'})
>>> s.text
'{\n  "cookies": {\n    "from": "my=browser"\n  }\n}\n'

</pre>

#### 设置超时

当发起一个请求遇到服务器响应非常缓慢而你又不希望等待太久时，可以指定 timeout 来设置请求超时时间，单位是秒，超过该时间还没有连接服务器成功时，请求将强行终止。

<pre>

>>> r = requests.get('https://google.com', timeout=5)

</pre>

#### 设置代理

一段时间内发送的请求太多容易被服务器判定为爬虫，所以很多时候我们使用代理IP来伪装客户端的真实IP。

<pre>

import requests

proxies = {
    'http': 'http://127.0.0.1:1080',
    'https': 'http://127.0.0.1:1080',
}

r = requests.get('http://www.kuaidaili.com/free/', proxies=proxies, timeout=2)

</pre>

#### Session

如果想和服务器一直保持登录（会话）状态，而不必每次都指定 cookies，那么可以使用 session，Session 提供的API和 requests 是一样的。

<pre>

>>> import requests
>>> s = requests.Session()
>>> s.cookies = requests.utils.cookiejar_from_dict({"a": "c"})
>>> r = s.get('http://httpbin.org/cookies')
>>> print(r.text)
{
  "cookies": {
    "a": "c"
  }
}

>>> r = s.get('http://httpbin.org/cookies')
>>> print(r.text)
{
  "cookies": {
    "a": "c"
  }
}

</pre>

### 2-小试牛刀

我们可以使用Requests完成一个爬取知乎专栏用户关注列表的简单爬虫。

用 Requests 模拟浏览器发送请求给服务器，写程序前，要先分析出这个请求是怎么构成的？请求URL是什么？请求头有哪些？查询参数有哪些？只有清楚了这些，你才好动手写代码，掌握分析方法很重要，否则一头雾水。

通过详细分析请求的构成，明确请求URL、请求方法、user-agent、查询参数，利用这些请求数据我们就可以用requests这个库来构建一个请求，通过Python代码来抓取这些数据。

<pre>

import requests

class SimpleCrawler:

    def crawl(self, params=None):
        # 必须指定UA，否则知乎服务器会判定请求不合法

        url = "https://www.zhihu.com/api/v4/columns/pythoneer/followers"
        # 查询参数
        params = {"limit": 20,
                  "offset": 0,
                  "include": "data[*].follower_count, gender, is_followed, is_following"}

        headers = {
            "authority": "www.zhihu.com",
            "user-agent": "Mozilla/5.0 (Windows NT 10.0; Win64; x64) "
                          "AppleWebKit/537.36 (KHTML, like Gecko) Chrome/63.0.3239.84 Safari/537.36",
        }
        response = requests.get(url, headers=headers, params=params)
        print("请求URL：", response.url)
        # 你可以先将返回的响应数据打印出来，拷贝到 http://www.kjson.com/jsoneditor/ 分析其结构。
        print("返回数据：", response.text)

        # 解析返回的数据
        for follower in response.json().get("data"):
            print(follower)

if __name__ == '__main__':
    SimpleCrawler().crawl()

</pre>

### 3-小结

这就是一个最简单的基于 Requests 的单线程知乎专栏粉丝列表的爬虫，requests 非常灵活，请求头、请求参数、Cookie 信息都可以直接指定在请求方法中，返回值 response 如果是 json 格式可以直接调用json()方法返回 python 对象。关于 Requests 的更多使用方法可以参考官方文档：http://docs.python-requests.org/en/master/

## 第三篇-抓包分析公众号请求过程

使用 Requests，配合 Chrome 浏览器可以实现了一个简单爬虫，但因为微信公众号的封闭性，微信公众平台并没有对外提供 Web 端入口，只能通过手机客户端接收、查看公众号文章，所以，为了窥探到公众号背后的网络请求，我们需要借助代理工具的辅助。

<div class="scale"><img src="img/resources/crawler/httpproxy.png"  alt="httpproxy" /></div>

HTTP代理工具又称为抓包工具，主流的抓包工具 Windows 平台有 Fiddler，macOS 有 Charles，阿里开源了一款工具叫 AnyProxy。它们的基本原理都是类似的，就是通过在手机客户端设置好代理IP和端口，客户端所有的 HTTP、HTTPS 请求就会经过代理工具，在代理工具中就可以清晰地看到每个请求的细节，然后可以分析出每个请求是如何构造的，弄清楚这些之后，我们就可以用 Python 模拟发起请求，进而得到我们想要的数据。

配置好抓包工具的几个步骤主要包括指定监控的端口，开通HTTPS流量解密功能，同时，客户端需要安装CA证书。

在用 Python 代码来模拟微信请求之前，先明确抓取数据的具体内容：

1、服务器的响应结果，200 表示服务器对该请求响应成功

2、请求协议，微信的请求协议都是基 于HTTPS 的，所以前面一定要配置好，不然你看不到 HTTPS 的请求。
3、微信服务器主机名

4、请求路径

5、请求行，包括了请求方法（GET），请求协议（HTTP/1.1），请求路径（/mp/profile_ext...后面还有很长一串参数） 

6、包括Cookie信息在内的请求头。

7、微信服务器返回的响应数据，我们分别切换成 TextView 和 WebView 看一下返回的数据是什么样的

通过抓包分析公众号请求过程，接着就可以基于Requests模拟像微信服务器发起请求。


<div class="scale"><img src="img/authors/wechatloi.jpg"  alt="wechatloi" /></div>



