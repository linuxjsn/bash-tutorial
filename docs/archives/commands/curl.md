curl 是 Linux 系统上一款网络工具，它的首字母 c 代表的是 client，表示它是客户端程序。通过 URL 方式，可以实现客户端与服务器之间传递数据。

它的功能非常强大，支持大部分常见的网络协议：HTTP、HTTPS、FTP。功能特性也很丰富，支持 http、https、cookie、认证、代理、速率限制、断点续传，提供非常多的选项，熟练运用的话，基本可以取代 postman 这类图形工具。

# 与liburl关系

---

其实 curl 项目包括两部分：<font color="#ff0000">curl</font> 和 <font color="#ff0000">libcurl</font>，关系大概如图所示

![curl和libcurl关系图](https://raw.githubusercontent.com/picjsn/pic/main/img/202205022251244.jpg)

- <font color="#ff0000">curl</font> 是命令行工具，底层调用的是  <font color="#ff0000">libcurl</font> 库。
-  <font color="#ff0000">libcurl</font> 是以**库**形式存在，提供各种功能  <font color="#ff0000">C</font> 接口，供其他程序调用，包括 <font color="#ff0000">curl</font> 命令

# 安装使用

---

和 wget 不同，并非所有的 Linux 发行版中都安装了 curl，你可以使用包管理器自行安装

```bash
# ubuntu or debian
$ apt install curl
# centos or redhat
$ yum install curl
```

如果需要使用最新版本，或自定义安装，可以通过源码编译方式进行安装

```bash
$ wget https://curl.se/download/curl-7.79.1.tar.gz
$ ./configure
$ make
$ make install
```

通过 <font color="#ff0000">curl --version</font> 可以验证命令是否安装准确

# 上手操作

我们先来看下 curl 最简单的使用方式，没有任何选项，将服务器响应的内容输出到屏幕上

```bash
$ curl http://linuxblogs.cn
```

有时我们不想显示错误和进度信息，可以使用 -s 选项开启静默模式

```bash
$ curl -s http://linuxblogs.cn
# 完全不输出任何内容，可以通过"echo $?"来判断命令成功或失败
$ curl -s -o /dev/null http://linuxblogs.cn
```

通过 -v 选项可以非常详细地显示 curl 的整个工作过程，相当于打开了调试模式

# 请求http

接着介绍我们平时最常用的，和 http 数据传输相关的操作

1. 发送 GET 请求

curl 命令默认发送的是 GET 请求，响应内容直接打印在了屏幕上

```bash
$ curl http://www.baidu.com
```

使用 -i 选项，可以打印服务器响应的 HTTP 头部信息

```bash
# 先打印请求头，空一行，再打印网页内容
$ curl -i http://www.baidu.com
```

如果只想测试该链接或资源是否正常，使用 -I 选项，可以只打印响应头信息，注意此时发送的是 HEAD 请求

2. 发送 POST 请求

默认情况下，curl发送的是GET请求，使用-X参数可以指定发送POST请求，使用-d参数可以指定请求数据

```bash
# 无数据的 POST 请求
$ curl -x POST http://www.domain.com
# 发送 Form 数据
$ curl -d 'user=foo&pass=123' -X POST http://google.com/login 
# 等价于上边命令
$ curl -d 'user=foo' -d 'pass=123' http://google.com/login
```

使用 -d 选项后，默认就是 POST 请求，可以省略 -X 选项，另外，使用多个 -d 选项，可以使命令行显得更清晰

下边命令可以读取本地文件，作为数据向服务器发送

```bash
$ curl -d '@data.txt' http://google.com/login
```

3. 发送Json格式数据请求

curl 可以发送 json 格式的请求，需要设置 Content-Type 为 application/json

```bash
$ curl -d '{"user":"foo","pass":"123"}' \
-H 'Content-Type: application/json' \
http://google.com/login
```

-H 选项指定 Content-Type 请求头为 json 格式，这样 web 服务器就清楚数据类型，知道该怎么处理了

4. 构造查询字符串参数

   通过 -G 选项，可以构造查询字符串参数

```bash
curl -G -d 'q=chopin' -d 'count=20' http://google.com/search
# 等价于下边命令
curl 'http://google.com/search?q=chopin&count=20'
```

上述命令会发送 GET 请求，如果忽略 -G 选项，会发出一个 POST 请求

5. 添加请求头

   通过 -H 选项，可以为请求添加标头

```bash
$ curl -H 'Accept-Language: en-US' http://google.com
# 可以指定多个-H选项
$ curl -H 'Accept-Language: en-US' -H 'Secret-Message: xyzzy' http://google.com
```

6. 设置重定向

   默认 curl 不会跟随重定向，指定 -L 选项会让请求跟随服务器重定向

