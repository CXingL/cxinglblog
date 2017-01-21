---
title: MarkDown 学习笔记
date: 2017-01-12 15:08:59
tags: [MarkDown, zhuozhuo, study]
---


本 blog 发布内容使用 markdown，在此先学习 markdown 的主要语法，并记录，方便以后查看。
上为代码，下为 demo（因为有标题示例，所以文章目录是混乱的）

<!-- more -->

### 标题

```
This is an H1
=============
```
This is an H1
=============

```
This is an H2
-------------
```
This is an H2
-------------

```
# 这是 H1
## 这是 H2
### 这是 H3
#### 这是 H4
##### 这是 H5
###### 这是 H6
这是正文
```
# 这是 H1
## 这是 H2
### 这是 H3
#### 这是 H4
##### 这是 H5
###### 这是 H6
这是正文

### 区块引用
###### 使用 > 引用
```
> This is a blockquote with two paragraphs. Lorem ipsum dolor sit amet,
> consectetuer adipiscing elit. Aliquam hendrerit mi posuere lectus.
```
> This is a blockquote with two paragraphs. Lorem ipsum dolor sit amet,
consectetuer adipiscing elit. Aliquam hendrerit mi posuere lectus.

###### 也可以整段只使用一个
```
> This is a blockquote with two paragraphs. Lorem ipsum dolor sit amet,
consectetuer adipiscing elit. Aliquam hendrerit mi posuere lectus.
```
> This is a blockquote with two paragraphs. Lorem ipsum dolor sit amet,
consectetuer adipiscing elit. Aliquam hendrerit mi posuere lectus.

###### 还可以嵌套
```
> first level of quroting
>>second lcvel of quoting
```
> first level of quroting
>>second lcvel of quoting

###### 引用内加上其它 MarkDown 语法
```
> ## 这是一个标题。
> 
> 1.   这是第一行列表项。
> 2.   这是第二行列表项。
> 
> 给出一些例子代码：
> 
>     return shell_exec("echo $input | $markdown_script");
```
> ## 这是一个标题。
> 
> 1.   这是第一行列表项。
> 2.   这是第二行列表项。
> 
> 给出一些例子代码：
> 
>     return shell_exec("echo $input | $markdown_script");

### 列表
MarkDown 支持有序列表和无序列表

###### 无序列表使用 * + - 作为列表标记
```
*   red
*   green
*   blue
```
*   red
*   green
*   blue

###### 有序列表使用数字+点
```
1.    bird
2.    mchale
3.    parish
```
1.    bird
2.    mchale
3.    parish

###### 数字不会影响输出
```
1.    bird
1.    mchale
4.    parish
```
1.    bird
1.    mchale
4.    parish

###### 使用固定的缩进
```
*   Lorem ipsum dolor sit amet, consectetuer adipiscing elit.
    Aliquam hendrerit mi posuere lectus. Vestibulum enim wisi,
    viverra nec, fringilla in, laoreet vitae, risus.
*   Donec sit amet nisl. Aliquam semper ipsum sit amet velit.
    Suspendisse id sem consectetuer libero luctus adipiscing.
```
*   Lorem ipsum dolor sit amet, consectetuer adipiscing elit.
    Aliquam hendrerit mi posuere lectus. Vestibulum enim wisi,
    viverra nec, fringilla in, laoreet vitae, risus.
*   Donec sit amet nisl. Aliquam semper ipsum sit amet velit.
    Suspendisse id sem consectetuer libero luctus adipiscing.

###### 不使用
```
*   Lorem ipsum dolor sit amet, consectetuer adipiscing elit.
Aliquam hendrerit mi posuere lectus. Vestibulum enim wisi,
viverra nec, fringilla in, laoreet vitae, risus.
*   Donec sit amet nisl. Aliquam semper ipsum sit amet velit.
Suspendisse id sem consectetuer libero luctus adipiscing.
```
*   Lorem ipsum dolor sit amet, consectetuer adipiscing elit.
Aliquam hendrerit mi posuere lectus. Vestibulum enim wisi,
viverra nec, fringilla in, laoreet vitae, risus.
*   Donec sit amet nisl. Aliquam semper ipsum sit amet velit.
Suspendisse id sem consectetuer libero luctus adipiscing.

###### 如果要放代码缩进 2 次，8 个空格或者 2 个制表符
```
*    列表包含代码块
         renturn true
```
*    列表包含代码块
         renturn true

###### 反斜杠防止产生列表
```
2017. what a great year

2017\. what a great year
```
2017. what a great year

2017\. what a great year

### 代码区块
在 MarkDown 中放置已经排版好的代码

###### 使用 4 个空格或者 1 个制表符
```
this is normal

    this is code
```
this is normal

    this is code


### 分隔线
可以在一行中用三个以上的星号、减号、底线来建立一个分隔线，行内不能有其他东西。你也可以在星号或是减号中间插入空格

    * * *

    ***

    *****

    - - -

    ---------------------------------------
* * *

***

*****

- - -

---------------------------------------

### 区段元素
#### 链接
##### 行内式
只要在方块括号后面紧接着圆括号并插入网址链接即可，如果你还想要加上链接的 title 文字，只要在网址后面，用双引号把 title 文字包起来

    This is [an example](http://example.com/ "Title") inline link.
    [This link](http://example.net/) has no title attribute.

This is [an example](http://example.com/ "Title") inline link.
[This link](http://example.net/) has no title attribute.

###### 同样主机可以使用相对路径

    See my [About](/about/) page for details.

See my [About](/about/) page for details.

##### 参考式
在链接文字的括号后面再接上另一个方括号，而在第二个方括号里面要填入用以辨识链接的标记

    This is [an example][id] reference-style link.

This is [an example][id] reference-style link.

然后在文件的任意处，把这个标记的链接定义出来

    [id]: http://example.com/  "Optional Title Here"

[id]: http://example.com/  "Optional Title Here"

链接的内容定义的形式为：
- 方括号（前面可以选择性地加上至多三个空格来缩进），里面输入链接文字
- 接着一个冒号
- 接着一个以上的空格或制表符
- 接着链接的网址
- 选择性地接着 title 内容，可以用单引号、双引号或是括弧包着

也可以使用链接名称的方式

    I get 10 times more traffic from [Google][] than from
    [Yahoo][] or [MSN][].

      [google]: http://google.com/        "Google"
      [yahoo]:  http://search.yahoo.com/  "Yahoo Search"
      [msn]:    http://search.msn.com/    "MSN Search"
I get 10 times more traffic from [Google][] than from [Yahoo][] or [MSN][].

  [google]: http://google.com/        "Google"
  [yahoo]:  http://search.yahoo.com/  "Yahoo Search"
  [msn]:    http://search.msn.com/    "MSN Search"

#### 强调
###### 使用星号和下滑线作为标记强调字词的符号

    *single asterisks*

    _single underscores_

    **double asterisks**

    __double underscores__
*single asterisks*

_single underscores_

**double asterisks**

__double underscores__

如果你的 * 和 _ 两边都有空白的话，它们就只会被当成普通的符号

#### 代码
###### 要标记一小段行内代码，你可以用反引号把它包起来

    Use the `printf()` function.
Use the `printf()` function.

###### 如果要在代码区段内插入反引号，你可以用多个反引号来开启和结束代码区段
    ``There is a literal backtick (`) here.``
``There is a literal backtick (`) here.``

#### 图片
###### 行内式图片语法
    ![Alt text](/path/to/img.jpg)

    ![Alt text](/path/to/img.jpg "Optional title")
![Alt text](/picture.ico)

![Alt text](/picture.ico "Optional title")
详细使用方法：
- 一个惊叹号 !
- 接着一个方括号，里面放上图片的替代文字
- 接着一个普通括号，里面放上图片的网址，最后还可以用引号包住并加上 选择性的 'title' 文字
###### 参考式图片语法：
``![Alt text][id]``
![Alt text][id]
    [id]: url/to/image  "Optional title attribute"
[id]: /picture.ico  "Optional title attribute"

#### 自动链接
Markdown 支持以比较简短的自动链接形式来处理网址和电子邮件信箱，只要是用尖括号包起来， Markdown 就会自动把它转成链接。一般网址的链接文字就和链接地址一样
``<http://example.com/>
<address@example.com>``
<http://example.com/>
<address@example.com>

#### 反斜杠
利用反斜杠来插入一些在语法中有其它意义的符号
`    \*abcd\* `
\*abcd\*

以下斜杠支持反斜杠
```
    \   反斜线
    `   反引号
    *   星号
    _   底线
    {}  花括号
    []  方括号
    ()  括弧
    #   井字号
    +   加号
    -   减号
    .   英文句点
    !   惊叹号
```

### 表格
竖线和横线来构造表格
```
| Tables        | Are           | Cool  |
| ------------- |:-------------:| -----:|
| col 3 is      | right-aligned | $1600 |
| col 2 is      | centered      |   $12 |
| zebra stripes | are neat      |    $1 |
```
| Tables        | Are           | Cool  |
| ------------- |:-------------:| -----:|
| col 3 is      | right-aligned | $1600 |
| col 2 is      | centered      |   $12 |
| zebra stripes | are neat      |    $1 |