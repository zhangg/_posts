title: urlparse+urlliab*介绍
date: 2015-09-19 15:18:18
categories: python
tags: [python web]
---

简要的介绍python web编程中url*系列模块的用法
<!--more-->
web地址的格式：
`prot_sch://net_loc/path;params?query#flag`

其中net_loc可以进一步拆分成多个部分：`user:passwd@host:port`，基本上只有FTP连接才使用user和passwd，port也通常使用默认端口，因此我们平常见到的URL的net_loc其实就只有host了

---
url部件 | 描述
---|---
prot_sch | 网络协议或者下载规则
net_loc|服务器位置（或许有用户信息）
path|限定文件或者CGI应用程序路径
params|可选参数
query|连接符（&）连接键值对
frag|拆分文档中的特殊锚

python中处理url的有两个模块，一个是`urlparse`，一个是`urllib`(现在一般用的是他的升级版本`urllib2`)

##urlparse 模块
urlparse模块有三个功能（函数）：`urlparse()`，`urlunparse`，`urljoin`，这三个函数都非常简单。
**`urlparse()`** 将URL字符串拆分成上面所述的url部件，语法结构：
`urlparse(urlstr, defProtSch=None, allowFrag=None)`

给出一个示例：
```python
In [19]: urlparse.urlparse("http://www.python.org/doc/FAQ.html")
Out[19]: ParseResult(scheme='http', netloc='www.python.org', path='/doc/FAQ.html', params='', query='', fragment='')
```

**`urlunparse()`**与`urlparse`功能相反，下面的的等式是成立的：`urlunparse(urlparse(urlstr)) == urlstr`

**`urljoin()`**的功能也很直观，其语法为
`join(baseurl, newurl, allowFrag=None)`

给出一个例子：
```python
In [20]: urlparse.urljoin('http://www.baidu.com/doc/a.html', 'aaa/bbb/ccc/html')
Out[20]: 'http://www.baidu.com/doc/aaa/bbb/ccc/html'
```

##urllib模块
url模块提供了你通过url访问资源的所有操作。主要的操作包含如下的6部分。
###urlopen()
`urlopen()`打开一个给定URL字符串与web连接，并返回文件类的对象。语法结构：
`urlopen(urlstr, postQueryData=None)
如果使用POST方法，请求的字符串应该被放到postQueryData中）
`
连接成功后，会返回一个文件类型对象，我们可以像操作一个本地文件一样对它进行操作：

urlopen()对象方法 | 描述
---|---
f.read([bytes)] |取出所有或者bytes个字节
f.readline() | 读出一行
f.readlines()|读出所有行并返回一个列表
f.close()|关闭f的url连接
f.fileno()|关闭f的文件句柄
f.info|获得f的MIME头文件（多目标因特网右键扩展），它可以通知浏览器返回的 文件类型可以用哪类应用打开
f.geturl|返回f打开的正真URL

###urlretrieve()
`urlretrieve()`可以方便的将urlstr定位到的整个web页面下载带本地。然后你可以对下载好的内容进行任何操作。语法如下：
`urllib.urlretrieve(url[, filename[, reporthook[, data]]])`
它返回的是一个二元组（filename，mime_hdrs）

###quote()和quote_plus()
它获取urlstr的数据，并将其编码（其实就是替换）。字母、数字、‘-  . _’，这些不需要替换，其他的祖父都被替换成`%xx`这样的格式。可选的`safe`参数指定其他不需要替换的字符，默认的是'/'。
`urllib.quote(string[, safe])`

`quote_plus()`和`quote()`功能几乎一样，除了会把urlstring中的空格替换成‘+’。
```python
In [2]: url = 'http://www.baidu.com/~foo/s.py?name=aaa xxx=bbb'
In [3]: urllib.quote(url)
Out[3]: 'http%3A//www.baidu.com/%7Efoo/s.py%3Fname%3Daaa%20xxx%3Dbbb'
In [4]: urllib.quote_plus(url)
Out[4]: 'http%3A%2F%2Fwww.baidu.com%2F%7Efoo%2Fs.py%3Fname%3Daaa+xxx%3Dbbb'
In [5]: urllib.quote_plus(url,'/')
Out[5]: 'http%3A//www.baidu.com/%7Efoo/s.py%3Fname%3Daaa+xxx%3Dbbb'
```

###unquote()和unquote_plus()
他们的作用和quote*相反，其语法如下：
`urllib.unquote(string)
urllib.unquote_plus(string)
`

###urlencode()
该函数可以将一个字典里面的key/value pair，编码成在url里面使用的参数赋值形式。可以看如下的例子：
```python
In [6]: adict = {'a':'111','b':'222','c':'333'}
In [7]: urllib.urlencode(adict)
Out[7]: 'a=111&c=333&b=222'
```

##urllib2模块
urllib2可视为urllib的加强版，增加了一些urllib所不能的一些功能。需要注意的是，在python3中，urllib2被分为了两个模块`urllib.request`和`urllib.error`。

