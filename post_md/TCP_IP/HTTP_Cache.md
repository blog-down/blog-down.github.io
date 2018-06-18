# HTTP协议缓存机制梳理
## 1. HTTP缓存机制概述
### 1.1 缓存的目的
HTTP缓存的目的是客户端或中介（代理服务器，CDN等）重用已获取的资源能够有效的提升网站与应用的性能。Web 缓存能够减少延迟与网络阻塞，进而减少显示某个资源所用的时间。
### 1.2 进行缓存的客户端
所有通过HTTP协议从服务器获取数据的节点都可看做服务的客户端，但是通过服务器和终端客户端之间是有第三者（例如代理服务器），即HTTP缓存机制是覆盖所有从HTTP服务器获取数据的客户端。
** 公用缓存： **
公用缓存是指缓存的数据是公用的，供多个客户端取用，最为典型的就是CDN的缓存。
** 私有缓存： **
私有缓存是指缓存的数据是私有的，只供单个客户端使用，最为典型的就是浏览器的缓存。

## 2. HTTP协议如何实现缓存


## 3. HTTP 1.0缓存机制

## 4. HTTP 1.1缓存机制

## 5. HTTP 1.0/1.1 混合使用缓存头

## 6. 其它
