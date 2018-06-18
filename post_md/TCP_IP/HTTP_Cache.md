<style>
html{
margin:0px 18% 20px 18%;
min-width:500px;
}
ul{
list-style:none;
}
</style>

# HTTP协议缓存机制梳理
June 18, 2018 4:01 PM
**目录**

[TOC]

## 1. HTTP缓存机制概述
### 1.1 缓存的目的
HTTP缓存的目的是客户端或中介（代理服务器，CDN等）重用已获取的资源能够有效的提升网站与应
用的性能。Web 缓存能够减少延迟与网络阻塞，进而减少显示某个资源所用的时间。
### 1.2 缓存的类型

<center>
<img src="https://blog-down.github.io/post_md/TCP_IP/HTTP_Cache/private_public_cache.png" style="width:600px;" />
<text style="font-size:x-small">图 1.1 私有缓存VS公共缓存</text>
</center>

所有通过HTTP协议从服务器获取数据的节点都可看做服务的客户端，但是通过服务器和终端客户端之间是有第三者（例如代理服务器），即HTTP缓存机制是覆盖所有从HTTP服务器获取数据的客户端。
** 公用缓存： **
公用缓存是指缓存的数据是公用的，供多个客户端取用，最为典型的就是_CDN的缓存_。
** 私有缓存： **
私有缓存是指缓存的数据是私有的，只供单个客户端使用，最为典型的就是_浏览器的缓存_。
## 2. HTTP协议如何实现缓存
### 2.1 HTTP缓存协商

<center>
<img src="https://blog-down.github.io/post_md/TCP_IP/HTTP_Cache/http_cache_talk.png" style="width:600px;" />
<text style="font-size:x-small">图 2.1 缓存协商</text>
</center>

HTTP协议对缓存机制的控制是通过 HTTP请求头和响应头来实现的，其中，请求头来自客户端，响应头来自服务器。也就是说，客户端和服务器之间通过_HTTP缓存相关的头部_来进行协商当前资源的缓存策略。
### 2.2 缓存的 Key
当HTTP响应被缓存在客户端后，当获取缓存内容时，客户端会比较当前的请求和被缓存内容的Key是否一致，以此判断是否使用缓存（如不使用，则请求服务器）。
Key通常是由一系列关键字组成的字符串，主要包括：
```
协议、协议版本、域、端口、路径、请求头字段、Cookie
```
上面列出的是常用的Key可能的关键字，但是每个客户端的Key都是由自己决定的，在缓存协商中，服务器不会告诉客户端如何去匹配缓存。不同的浏览器/CDN/代理服务器可能会使用不同的Key。
## 3. HTTP 1.0缓存机制
HTTP 1.0时代的缓存特别简单，它定义了两个头字段来进行缓存协商。

<center>
<img src="https://blog-down.github.io/post_md/TCP_IP/HTTP_Cache/http1.0_cache.png" style="width:600px;" />
<text style="font-size:x-small">图 3.1 HTTP1.0缓存机制</text>
</center>

** Pragma **: 这个字段只有一个值（no-cache）；当使用此字段时，表示禁用缓存。
** Expires **: 这个字段也只有一个值，缓存过期时间，它是一个绝对的时间值，例如 Wed, 31 May 2017 03:21:09 GMT ，当系统时间大于这个时间，则缓存过期了，不能使用。

** 场景 **：
1. 不使用缓存，服务器则定义Pragma:no-cache；客户端发起请求，但不使用公用缓存，则客户端请求头定义Pragma:no-cache。
2. 使用缓存，服务器响应则定义Expires: [date值]。

## 4. HTTP 1.1缓存机制

HTTP 1.1缓存机制较1.0版本比较复杂一些 ，但是设计却更加巧妙。1.1版本添加了新鲜度验证机制，通过资源的Etag（由资源的字节计算出来的key）或最近修改时间来验证资源是否和服务器一致，以此判断是否使用缓存和更新缓存信息。注意，新鲜度验证是很小的网络通信，若是验证失败，服务端才会返回完整的资源。

<center>
<img src="https://blog-down.github.io/post_md/TCP_IP/HTTP_Cache/http1.1_cache.png" style="width:800px;" />
<text style="font-size:x-small">图 4.1 HTTP1.1缓存机制</text>
</center>

HTTP1.1用于缓存协商的头字段分为两类，一类是定义缓存协商的头，一类是用于验证缓存是否有效的头。
其中，定义缓存协商的头只有一个 Cache-Control ，此字段的值非常多。此字段的某些值可同时运用于请求和响应。

<center><text style="font-size:x-small">块 4.1 用于响应的Cache-Control值</text></center>

```
Cache-control: must-revalidate
Cache-control: no-cache
Cache-control: no-store
Cache-control: no-transform
Cache-control: public
Cache-control: private
Cache-control: proxy-revalidate
Cache-Control: max-age=<seconds>
Cache-control: s-maxage=<seconds>
```

<center><text style="font-size:x-small">块 4.2 用于请求的Cache-Control值</text></center>

```
Cache-Control: max-age=<seconds>
Cache-Control: max-stale[=<seconds>]
Cache-Control: min-fresh=<seconds>
Cache-control: no-cache
Cache-control: no-store
Cache-control: no-transform
Cache-control: only-if-cached
```

<center><text style="font-size:x-small">块 4.3 用于新鲜度验证的头</text></center>

```
Etag
Last-Modified
If-Match
If-None-Match
If-Modified-Since
If-Unmodified-Since
```

Note: HTTP1.1 缓存头详尽信息查阅mdn和rfc文档
[https://tools.ietf.org/html/rfc2616](https://tools.ietf.org/html/rfc2616)

## 5. HTTP 1.0/1.1 混合使用缓存头
目前大多数服务器都会同时定义HTTP1.0/1.1缓存，目的是向下兼容浏览器。
当HTTP1.0内部缓存头冲突，1.1内部缓存头冲突，1.0和1.1缓存头冲突时，定义的缓存策略有效规则为：
1. 以最为严格的策略优先（倾向于不使用缓存或验证新鲜度）
2. 以HTTP1.1优先


## 6. 其它

1. 建议放弃在HTML中meta部分设置缓存相关信息，大部分浏览器放弃支持meta上的缓存设置
2. 对于相同的缓存策略，不同浏览器和CDN处理规则不一致，但是都会尽量减小网络通信（能使用缓存就尽量使用，例如默认未定义缓存策略，Chrome仍会缓存）
3. HTTP1.0的Expires字段并不准确，因为服务器和客户端日期可能不一致，所以HTTP1.1采用max-age和s-maxage相对的秒数来判定是否过期。
