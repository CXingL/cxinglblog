---
title: pyhton 爬虫入门
date: 2017-01-14 14:06:50
tags: [python, study, zhuozhuo]
---

学习 python 的爬虫

## Urllib 基础用法

### 1. 最简单的扒一个网页
    >>> import urllib2
    >>> response = urllib2.urlopen('http://www.baidu.com')
    >>> print response.read()

<!-- more -->

urlopen 是 urllib2 里的一个方法，接收三个参数  `urlopen(url, data, timeout)`
第一个参数 url 即为URL，第二个参数 data 是访问 URL 时要传送的数据，第三个 timeout 是设置超时时间。
第二三个参数是可以不传送的，data 默认为空 None，timeout 默认为 socket._GLOBAL_DEFAULT_TIMEOUT
第一个参数URL是必须要传送的，传送了百度的URL，执行urlopen方法之后，返回一个response对象，返回信息便保存在其中

### 2. 构造 Request
传入一个 request 请求，通过中间多一个 request 对象，在逻辑上更加清晰明确
    >>> import urllib2
    >>> request = urllib2.Request('http://www.baidu.com')
    >>> response = urllib2.urlopen(request)
    >>> print response.read()
运行结果和上边应该是一样的
### 3. POST 和 GET 数据传送
大多数网站都是都太网页，需要传递参数后网站做出回应，例如登录和注册。
**数据传递分为 POST 和 GET 两种方式**
GET 是直接以链接形式访问，链接包含了所有参数，可以直观的看到自己提交了什么内容，包括密码
POST 则不会在网址上显示所有的参数
**POST 方式**
data 参数传入数据
    >>> import urllib
    >>> import urllib2
    >>> values = {'username': 'a', 'password': 'a'}
    >>> data = urllib.urlencode(values)
    >>> url = 'https://passport.csdn.net/account/login?from=http://my.csdn.net/my/mycsdn'
    >>> request = urllib2.Request(url, data)
    >>> response = urllib2.urlopen(request)
    >>> print response.read()
定义了叫 values 的字典，传入参数 username 和 password，然后利用 urllib.encode 方法将字典编码，命名为 data，然后传入 url，返回 POST 后的内容
**GET 方式**
    >>> import urllib
    >>> import urllib2
    >>> values = {}
    >>> values['username'] = 'a'
    >>> values['password'] = 'a'
    >>> data = urllib.urlencode(values)
    >>> url = 'http://passport.csdn.net/account/login'
    >>> geturl = url + '?' + data
    >>> request = urllib2.Request(geturl)
    >>> response = urllib2.urlopen(request)
    >>> print response.read()

---------- update: 2017年01月14日18:00 ----------

## Urllib 高级用法

### 设置 Headers
有些网站不允许程序使用上表面的方式访问，所以为了完全模拟浏览器的工作，需要设置一些 Headers 的属性
打开新的网页时，会有许多请求，一个请求中又会有 Request URL，headers，response等等。
headers 中包含的是很多信息，有文件编码，压缩方式，请求的 agent 等等

agent 是请求的身份，如果没有写入请求的身份，那么服务器不一定会响应，所以可以在 headers 中设置 agent，例如：

    import urllib
    import urllib2

    url = 'http://www.server.com/login'
    use_agent = 'Mozilla/4.0(compatible; MSIE 5.5; Windows NT)'
    values = {'username': 'cqc', 'password': 'xxxx'}
    headers = {'User-Agent': user_agent}
    data = urllib.urlencode(values)
    request = urllib2.Request(url, data, headers)
    response = urllib2.urlopen(request)
    page = response.read()

以上，设置了一个 headers，在构建 request 时传入，在请求时，就加入了 headers 传送，服务器识别了是浏览器发来的请求，就回得到响应。
**对付’反盗链‘**
对付防盗链，服务器会识别 headers 中的 referer 是不是它自己，如果不是，有的服务器不会响应，所以可以在 headers 中加入 referer
例如可以这样构建 headers：

    headers = {'User-Agent': 'Mozilla/4.0(compatible; MSIE 5.5; Windows NT)'. 'Referer': 'http://www.zhihu.com/articles'}

这样就可以应付防盗链了

另外 headers 的一些属性，一下需要特别注意：

> User-Agent: 有些服务器或 Proxy 会通过该值来判断是否是浏览器发出的请求
> Content-Type: 在使用 REST 接口时，服务器会检查该值，用来确定 HTTP Body 中的内容该怎样解析
> application/xml：在 XML RPC, 如 RESTful/SOAP 调用时使用
> application/json：在 JSON RPC 调用时使用
> application/x-www-form-urlencoded: 浏览器提交 Web 表单时使用
> 在使用服务器提供的 RESTful 或 SOAP 服务时，Content-Type 设置错误会导致服务器拒绝服务

### Proxy（代理）的设置
urllib2 默认使用环境变量 http_proxy 来设置 HTTP Proxy。如果网站检测某一时段某个 IP 的访问次数，访问过多则禁止访问，可以设置一些代理服务器来帮助工作，每隔一段时间换一个代理
代理的设置用法：

    import urllib2
    enable_proxy =  True
    proxy_handler = urllib2.ProxyHandler({"http": 'http://some-proxy.com:8000'})
    null_proxy_handler = urllib2.ProxyHandler({})
    if enable_proxy:
        opener = urllib2.build_opener(proxy_handler)
    else:
        opener = urllib2.build_opener(null_proxy_handler)
    urllib2.install_opener(opener)

### Timeout 设置
最开始使用过的 urlopen 方法，第三个参数就是 timeout 的设置，可以设置超时时间，解决一些网站响应过慢的影响
如果第二个参数 data 为空，要特别指定是 timeout 是多少，写明形参，如果 data 已经传入，则可以不声明：

    import urllib2
    response = urllib2.urlopen('http://www.baidu.com', timeout = 10)
and

    import urllib2
    response = urllib2.urlopen('http://www.baidu.com', data, 10)

### URLError 异常处理
URLError 可能产生的原因：
- 网络无连接，本机无法上网
- 连接不到特定的服务器
- 服务期不存在
在代码中，我们用 try-except 语句包围并捕获相应的异常：

    import urllib2

    requset = urllib2.Request('http://www.xxxx.com')
    try:
        urllib2.urlopen(request)
    except urllib2.URLError, e:
        print e.reason
访问一个不存在的网址，运行结果如下：

    [Errno 11004] getaddrinfo failed
表示：错误代号是 11004，错误原因是 getaddrinfo failed

---------- update: 2017年01月20日18:17 ----------