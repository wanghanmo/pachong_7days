# Task1 学习get与post请求
## get与post
- GET在浏览器回退时是无害的，而POST会再次提交请求。
- GET产生的URL地址可以被Bookmark，而POST不可以。
- GET请求会被浏览器主动cache，而POST不会，除非手动设置。
- GET请求只能进行url编码，而POST支持多种编码方式。
- GET请求参数会被完整保留在浏览器历史记录里，而POST中的参数不会被保留。
- GET请求在URL中传送的参数是有长度限制的，而POST么有。
- 对参数的数据类型，GET只接受ASCII字符，而POST没有限制。 
- GET比POST更不安全，因为参数直接暴露在URL上，所以不能用来传递敏感信息。
- GET参数通过URL传递，POST放在Request body中。

  简单来说，对于GET方式的请求，浏览器会把http header和data一并发送出去，服务器响应200（返回数据）；而对于POST，浏览器先发送header，服务器响应100 continue，浏览器再发送data，服务器响应200 ok（返回数据）。
### 代码实现
  这里在Python 3.6.5环境下用requests库里的get方法向[百度一下，你就知道](www.baidu.com)发出一个请求，并将其返回结果输出。 
```
import requests
print('访问baidu网站 获取Response对象')
r = requests.get("http://www.baidu.com")
print(r.status_code)
print(r.encoding)
print(r.apparent_encoding)
print('将对象编码转换成UTF-8编码并打印出来')
r.encoding = 'utf-8'
print(r.text)
```
  运行结果:
```
访问baidu网站 获取Response对象
200
ISO-8859-1
utf-8
将对象编码转换成UTF-8编码并打印出来
<!DOCTYPE html>
<!--STATUS OK--><html> <head><meta http-equiv=content-type content=text/html;charset=utf-8><meta http-equiv=X-UA-Compatible content=IE=Edge><meta content=always name=referrer><link rel=stylesheet type=text/css href=http://s1.bdstatic.com/r/www/cache/bdorz/baidu.min.css><title>百度一下，你就知道</title></head> <body link=#0000cc> <div id=wrapper> <div id=head> <div class=head_wrapper> <div class=s_form> <div class=s_form_wrapper> <div id=lg> <img hidefocus=true src=//www.baidu.com/img/bd_logo1.png width=270 height=129> </div> <form id=form name=f action=//www.baidu.com/s class=fm> <input type=hidden name=bdorz_come value=1> <input type=hidden name=ie value=utf-8> <input type=hidden name=f value=8> <input type=hidden name=rsv_bp value=1> <input type=hidden name=rsv_idx value=1> <input type=hidden name=tn value=baidu><span class="bg s_ipt_wr"><input id=kw name=wd class=s_ipt value maxlength=255 autocomplete=off autofocus></span><span class="bg s_btn_wr"><input type=submit id=su value=百度一下 class="bg s_btn"></span> </form> </div> </div> <div id=u1> <a href=http://news.baidu.com name=tj_trnews class=mnav>新闻</a> <a href=http://www.hao123.com name=tj_trhao123 class=mnav>hao123</a> <a href=http://map.baidu.com name=tj_trmap class=mnav>地图</a> <a href=http://v.baidu.com name=tj_trvideo class=mnav>视频</a> <a href=http://tieba.baidu.com name=tj_trtieba class=mnav>贴吧</a> <noscript> <a href=http://www.baidu.com/bdorz/login.gif?login&amp;tpl=mn&amp;u=http%3A%2F%2Fwww.baidu.com%2f%3fbdorz_come%3d1 name=tj_login class=lb>登录</a> </noscript> <script>document.write('<a href="http://www.baidu.com/bdorz/login.gif?login&tpl=mn&u='+ encodeURIComponent(window.location.href+ (window.location.search === "" ? "?" : "&")+ "bdorz_come=1")+ '" name="tj_login" class="lb">登录</a>');</script> <a href=//www.baidu.com/more/ name=tj_briicon class=bri style="display: block;">更多产品</a> </div> </div> </div> <div id=ftCon> <div id=ftConw> <p id=lh> <a href=http://home.baidu.com>关于百度</a> <a href=http://ir.baidu.com>About Baidu</a> </p> <p id=cp>&copy;2017&nbsp;Baidu&nbsp;<a href=http://www.baidu.com/duty/>使用百度前必读</a>&nbsp; <a href=http://jianyi.baidu.com/ class=cp-feedback>意见反馈</a>&nbsp;京ICP证030173号&nbsp; <img src=//www.baidu.com/img/gs.gif> </p> </div> </div> </div> </body> </html>
```
断开Internet后，代码运行报错`socket.gaierror: [Errno 11001] getaddrinfo failed`.
错误代码通常URLError在没有网络连接(没有路由到特定服务器)，或者服务器不存在的情况下产生。这种情况下，异常同样会带有"reason"属性，它是一个tuple（可以理解为不可变的数组），包含了一个错误号和一个错误信息。

### 什么是请求头，如何添加请求头？
  HTTP客户程序（例如浏览器），向服务器发送请求的时候必须指明请求类型（一般是GET或者 POST）。如有必要，客户程序还可以选择发送其他的请求头。大多数请求头并不是必需的，但Content-Length除外。对于POST请求来说 Content-Length必须出现。
　下面是一些最常见的请求头：
- Accept：浏览器可接受的MIME类型。
- Accept-Charset：浏览器可接受的字符集。
- Accept-Encoding：浏览器能够进行解码的数据编码方式，比如gzip。Servlet能够向支持gzip的浏览器返回经gzip编码的HTML页面。许多情形下这可以减少5到10倍的下载时间。
- Accept-Language：浏览器所希望的语言种类，当服务器能够提供一种以上的语言版本时要用到。
- Authorization：授权信息，通常出现在对服务器发送的WWW-Authenticate头的应答中。
- Connection：表示是否需要持久连接。如果Servlet看到这里的值为“Keep- Alive”，或者看到请求使用的是HTTP 1.1（HTTP 1.1默认进行持久连接），它就可以利用持久连接的优点，当页面包含多个元素时（例如Applet，图片），显著地减少下载所需要的时间。要实现这一 点，Servlet需要在应答中发送一个Content-Length头，最简单的实现方法是：先把内容写入 ByteArrayOutputStream，然后在正式写出内容之前计算它的大小。
- Content-Length：表示请求消息正文的长度。
- Cookie：这是最重要的请求头信息之一
- From：请求发送者的email地址，由一些特殊的Web客户程序使用，浏览器不会用到它。
- Host：初始URL中的主机和端口。
- If-Modified-Since：只有当所请求的内容在指定的日期之后又经过修改才返回它，否则返回304“Not Modified”应答。
- Pragma：指定“no-cache”值表示服务器必须返回一个刷新后的文档，即使它是代理服务器而且已经有了页面的本地拷贝。
- Referer：包含一个URL，用户从该URL代表的页面出发访问当前请求的页面。
- User-Agent：浏览器类型，如果Servlet返回的内容与浏览器类型有关则该值非常有用。
- UA-Pixels，UA-Color，UA-OS，UA-CPU：由某些版本的IE浏览器所发送的非标准的请求头，表示屏幕大小、颜色深度、操作系统和CPU类型。

在爬虫的时候，如果不添加请求头，可能网站会阻止一个用户的登陆，此时我们就需要添加请求头来进行模拟伪装，使用python添加请求头方法如下：
```
headers={"User-Agent" : "Mozilla/5.0 (Windows; U; Windows NT 5.1; zh-CN; rv:1.9.1.6) ",
  "Accept" : "text/html,application/xhtml+xml,application/xml;q=0.9,*/*;q=0.8",
  "Accept-Language" : "en-us",
  "Connection" : "keep-alive",
  "Accept-Charset" : "GB2312,utf-8;q=0.7,*;q=0.7"}
r=requests.post("http://baike.baidu.com/item/火影忍者",headers=headers,allow_redirects=False)   #allow_redirects设置为重定向
r.encoding="UTF-8"
print r.url
#print r.text
print r.headers #响应头
print r.request.headers #请求头

```
 
 ## 参考
 - https://www.cnblogs.com/logsharing/p/8448446.html
 - https://www.jianshu.com/p/89ab535989a9
 
