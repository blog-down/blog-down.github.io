# HTTP协议缓存机制梳理
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
公用缓存是指缓存的数据是公用的，供多个客户端取用，最为典型的就是CDN的缓存。
** 私有缓存： **
私有缓存是指缓存的数据是私有的，只供单个客户端使用，最为典型的就是浏览器的缓存。
## 2. HTTP协议如何实现缓存
### 2.1 HTTP缓存协商

<center>
<img src="https://blog-down.github.io/post_md/TCP_IP/HTTP_Cache/http_cache_talk.png" style="width:600px;" />
<text style="font-size:x-small">图 2.1 缓存协商</text>
</center>

HTTP协议对缓存机制的控制是通过 HTTP请求头和响应头来实现的，其中，请求头来自客户端，响应头来自服务器。也就是说，客户端和服务器之间通过HTTP缓存相关的头部来进行协商当前资源的缓存策略。
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


## 5. HTTP 1.0/1.1 混合使用缓存头

## 6. 其它






<style>
body {
margin:0px 18% 10px 18%;
min-width:500px;
}
</style>