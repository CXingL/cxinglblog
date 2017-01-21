---
title: python 模块
date: 2017-01-13 10:51:49
tags: [python, study]
---

###### 熟悉 python 的模块（尤其是标准库）是十分重要的，大多数问题都可以简单快捷的使用它们解决。用这篇 blog 来记录了解过的模块。
- 2017年01月13日11:02 -   标准库

<!-- more -->

## 标准库
#### sys 模块
系统相关的信息模块 import sys

sys.argv是一个list,包含所有的命令行参数, argv[0] 是脚本本身路径
使用方法：
    #!/usr/bin/python
    # Filename: using_sys.py

    import sys

    print 'The command line arguments are:'
    for i in sys.argv:
        print i

    print '\n\nThe PYTHONPATH is', sys.path, '\n' 
运行这个脚本并传入参数

    python using_sys.py we are arguments
得到结果

    The command line arguments are:
    using_sys.py
    we
    are
    arguments

    The PYTHONPATH IS ’python 环境变量'


sys.stdout sys.stdin sys.stderr 分别表示标准输入输出,错误输出的文件对象. 
sys.stdin.readline() 从标准输入读一行 sys.stdout.write("a") 屏幕输出a 
sys.exit(exit_code) 退出程序 
sys.modules 是一个dictionary，表示系统中所有可用的module 
sys.platform 得到运行的操作系统环境 
sys.path 是一个list,指明所有查找module，package的路径. 
等等一些有关系统的信息

#### os 模块
操作系统的功能的模块 import os

os.sep 可以取代操作系统特定的路径分割符。
os.name字符串指示你正在使用的平台。比如对于Windows，它是'nt'，而对于Linux/Unix用户，它是'posix'。
os.getcwd()函数得到当前工作目录，即当前Python脚本工作的目录路径。
os.getenv()和os.putenv()函数分别用来读取和设置环境变量。
os.listdir()返回指定目录下的所有文件和目录名。
os.remove()函数用来删除一个文件。
os.system()函数用来运行shell命令。
os.linesep字符串给出当前平台使用的行终止符。例如，Windows使用'\r\n'，Linux使用'\n'而Mac使用'\r'。

###### os和os.path模块

os.curdir:返回但前目录（'.')
os.chdir(dirname):改变工作目录到dirname
os.path.isdir(name):判断name是不是一个目录，name不是目录就返回false
os.path.isfile(name):判断name是不是一个文件，不存在name也返回false
os.path.exists(name):判断是否存在文件或目录name
os.path.getsize(name):获得文件大小，如果name是目录返回0L
os.path.abspath(name):获得绝对路径
os.path.normpath(path):规范path字符串形式
os.path.split(name):分割文件名与目录（事实上，如果你完全使用目录，它也会将最后一个目录作为文件名而分离，同时它不会判断文件或目录是否存在）
os.path.splitext():分离文件名与扩展名
os.path.join(path,name):连接目录与文件名或目录
os.path.basename(path):返回文件名
os.path.dirname(path):返回文件路径

### 文件操作

打开文件 
f = open("filename", "r") r只读 w写 rw读写 rb读二进制 wb写二进制 w+写追加 
读写文件 
f.write("a") f.write(str) 写一字符串 f.writeline() f.readlines() 与下read类同 
f.read() 全读出来 f.read(size) 表示从文件中读取size个字符 
f.readline() 读一行,到文件结尾,返回空串. f.readlines() 读取全部，返回一个list. list每个元素表示一行，包含"\n"\ 
f.tell() 返回当前文件读取位置 
f.seek(off, where) 定位文件读写位置. off表示偏移量，正数向文件尾移动，负数表示向开头移动。 
where为0表示从开始算起,1表示从当前位置算,2表示从结尾算. 
f.flush() 刷新缓存 
关闭文件 
f.close() 

----------update：2017年01月13日18:15:36----------

### urllib 模块
Urllib 库是 python 编写爬虫的必备知识，使用 Urllib 来获取网页信息

###### urllib.urlopen(url[, data[, proxies]]): 打开一个 url，返回一个文件对象，可以对这个文件对象进行操作
    >>> import urllib
    >>> f = urllib.urlopen('http://www.baidu.com')
    >>> firstLine = f.readline()
    >>> firstLine
    '<!DOCTYPE html>\n'
urlopen 返回对象提供方法：
- read() , readline() ,readlines() , fileno() , close() ：这些方法的使用方式与文件对象完全一样
- info()：返回一个httplib.HTTPMessage对象，表示远程服务器返回的头信息
- getcode()：返回Http状态码。如果是http请求，200请求成功完成;404网址未找到
- geturl()：返回请求的url

###### urllib.urlretrieve(url[, filname[, reporthook[, data]]]): urlretrieve 方法将 url 定位到的 html 文件下载到本地硬盘中。如果不指定 filename，则会存为临时文件。urlretrieve() 返回一个二元组（filename, mine_hdrs)
临时存放：
    >>> import urllib
    >>> filename = urllib.urlretrieve('http://www.baidu.com')
    >>> type(filename)
    <type 'tuple'>
    >>> filename[0]
    '/var/folders/tm/w4rdv6rd6h92rxc0n2fs2fr80000gn/T/tmpVymO4G'
    >>> filename[1]
    <httplib.HTTPMessage instance at 0x10c3ceb48>
存为本地文件：
    >>> import urllib
    >>> filename = urllib.urlretrieve('http://wwww.baidu.com', filename = '/Users/NeverMore/Downloads/urllib.html')
    >>> type(filename)
    <type 'tuple'>
    >>> filename[0]
    '/Users/NeverMore/Downloads/urllib.html'
    >>> filename[1]
    <httplib.HTTPMessage instance at 0x10c76b320>
###### urllib.urlcleanup(): 清除由于 urllib.urlretrieve() 所产生的缓存
###### urllib.quote(url) 和 urllib.quote_plus(url)：将 url 数据获取以后，并将其编码，从而适用于 URL 字符串中，使其能被打印和被 web 服务器接收
    >>> import urllib
    >>> urllib.quote('http://www.baidu.com')
    'http%3A//www.baidu.com'
    >>> urllib.quote_plus('http://www.baidu.com')
    'http%3A%2F%2Fwww.baidu.com'
###### urllib.unquote(url) and urllib.unquote_plus(url): 与上一个相反
###### urllib.urlencode(query)：将 dict 或者包含两个元素的元组列表转换成 url 参数，例如字典{'name': 'dark-bull', 'age': 200}将被转换为 "name=dark-bull&age=200"
可以与 urlopen 结合实现 post 方法和 get 方法：
GET 方法：
    >>> import urllib
    >>> params = urllib.urlencode({'spam':1, 'eggs':2, 'bacon':0})
    >>> params
    'eggs=2&bacon=0&spam=1'
    >>> f = urllib.urlopen('http://python.org/query%s' % params)
    >>> print f.read()
POST 方法：
    >>> import urllib
    >>> parmas = urllib.urlencode({'spam':1, 'eggs':2, 'bacon':0}) 
    >>> f = urllib.urlopen('http://python.org/query', parmas)
    >>> f.read()

---------- update：2017年01月14日15:45 ----------

<!--### urllib2 模块

###### Proxy（代理） 的设置-->