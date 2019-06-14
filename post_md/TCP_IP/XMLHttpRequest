<style>
html{
margin:0px 18% 20px 18%;
min-width:500px;
}
ul{
list-style:none;
}
</style>

# XMLHttpRequest

June 14, 2019 3:14 PM

[TOC]

## 简介
基本上所有浏览器(IE只在IE7及以上)支持XMLHttpRequest类，Ajax作为一种客户端(浏览器)与服务器异步(这里的异步指数据与网页异步)进行数据交互的方案，XMLHttpRequest是其主要实现。即，JS使用XMLHttpRequest，即可实现浏览器与服务器异步进行数据交互。

本文档说明标准XMLHttpRequest属性、方法、事件，不包含各浏览器提供的非标准部分。

XMLHttpRequest作为浏览器提供的类，只需搞清楚其对象的属性、接口、事件的涵义，及其它们之间的基本关系，即可准确使用它。

本文部分地方以xhr简称XMLHttpRequest。

## 构造方法
**描述**: XMLHttpRequest只提供了 1 个标准构造方法 XMLHttpRequest()，且不带任何参数。
1. XMLHttpRequest()

## 属性
1. XMLHttpRequest.readyState
说明: readyState用于记录XMLHttpRequest对象运行的 5 个阶段，其值为 0-4
| 值 | 含义 |
|:--:|:--:|
| 0 | 通过XMLHttpRequest()已创建 XMLHttpRequest 对象，但未调用 open() 方法 |
| 1 | 已调用 open() 方法，未调用 send() 方法 |
| 2 | 已调用 send() 方法，且 HTTP 响应行和响应头可用 |
| 3 | 正在加载响应 body |
| 4 | XMLHttpRequest 请求和响应都结束，正常情况下响应数据可用 |

2. XMLHttpRequest.onreadystatechange
说明: XMLHttpRequest.readyState变化时(即readystatechange事件)回调函数，其值设置为一个函数对象
3. XMLHttpRequest.timeout
说明: 超时时间，即为XMLHttpRequest对象等待响应的最长时间，当超时后将直接结束本次请求，可以设置超时回调函数，在超时后做一些必要操作。
4. XMLHttpRequest.status
说明: 响应的状态码
5. XMLHttpRequest.statusText
说明: 响应的状态文本，例如: 200 通常对应 OK
6. XMLHttpRequest.responseURL
说明: 请求和响应的URL
7. XMLHttpRequest.responseText
说明: 响应体的纯文本内容
8. XMLHttpRequest.responseXML
说明: 以Dom对象返回内容，假如响应的实际内容是XML或HTML则能正则解析并得到Dom对象，否则响应null
9. XMLHttpRequest.responseType 和 XMLHttpRequest.response
说明: 预设置响应内容并解析(当知道服务器会响应json时，设置responseType为'json'，则response属性将是一个json对象)，通常responseType属性在xhr接收响应前就设置，设置后XMLHttpRequest.response被自动解析为指定类型的对象。
当响应为纯文本或xml或html时，可直接使用responseText、responseXML，更加简单。
| responseType值 | 含义 |
|:--:|:--:|
| "" | 默认值，DomString，即字符串 |
| "arraybuffer" | JS  ArrayBuffer对象 |
| "blob" | JS Blob对象 |
| "document" | XML或HTML对象 |
| "json" | Json对象 |
| "text" | 和默认""一样，文本字符串 |
10. XMLHttpRequest.withCredentials
说明: xhr同域请求时，此属性无效；xhr跨域请求时，这个属性用于指示是否允许xhr携带目的域的Cookie，默认false，为true时将携带Cookie到目的域服务器。(注意: xhr能否请求成功受[同源策略](https://zh.wikipedia.org/wiki/%E8%B7%A8%E4%BE%86%E6%BA%90%E8%B3%87%E6%BA%90%E5%85%B1%E4%BA%AB)影响)
11. XMLHttpRequest.upload
说明: 其值为一个文件上传对象，常用在需要回显文件上传进度的地方，一般的xhr用法不涉及这个对象(这里不做累述)。

## 方法
1. XMLHttpRequest.open()
XMLHttpRequest.open(method, url)
XMLHttpRequest.open(method, url, async)
XMLHttpRequest.open(method, url, async, user)
XMLHttpRequest.open(method, url, async, user, password)
method: 方法
url: 请求地址
async: 是否异步，默认true，设置为false将可能由于xhr等待响应导致页面卡住假死
user: 验证的用户名
password: 验证的密码
2. XMLHttpRequest.send()
XMLHttpRequest.send(body)
发送请求数据
3. XMLHttpRequest.setRequestHeader()
XMLHttpRequest.setRequestHeader(header, value)
在send() 方法执行前设置请求头
4. XMLHttpRequest.overrideMimeType()
XMLHttpRequest.overrideMimeType(mimeType)
用于重写服务器响应的错误 mimeType，mimeType设置在content-type响应头中，如果服务器设置的mimeType与实际数据类型不一致，可能导致xhr 不能正常解析响应数据，即xhr的XMLHttpRequest.responseXML等属性为null
5. XMLHttpRequest.getAllResponseHeaders() 和 XMLHttpRequest.getResponseHeader()
用于获取响应头
XMLHttpRequest.getResponseHeader(headerName)
XMLHttpRequest.getAllResponseHeaders()
6. XMLHttpRequest.abort()
用于中断当前xhr请求，通常在检查到异常时使用此方法中断请求(如检查到用户多次点击提交表单，则中断除第一次外的所有后续提交)

## 事件
**描述**: xhr的事件可通过两种方法设置回调函数，addEventListener()方法和on事件属性
通常直接使用on事件属性设置回调函数，因为简单易用，这里不做addEventListener()的累述

| 事件 | 描述 |
|:--:|:--:|
| abort | xhr被中断时触发，如xhr调用abort()方法时，对应属性 onabort |
| error | xhr发生error时触发，如服务器断开连接，对应属性 onerror |
| load | xhr完成请求和响应时触发，对应属性 onload |
| loadend | xhr结束时触发，包括正常请求响应结束和异常中断(如调用abort)，其在load事件触发后触发, 对应属性为 onloadend |
| loadstart | xhr开始加载响应数据时触发，在load和loadend前触发, 对应属性为 onloadstart |
| progress | xhr的progress接收到更多数据时触发，这里的接收到更多数据时应该是指经浏览器再次响应数据给xhr对象时，当下载一个大文件时，progress事件将触发多次; 同样的，XMLHttpRequest.upload对象也有progress事件，不过它是上传文件时触发，同样的，上传大文件时将触发多次，当上传大文件需要回显进度时，需要使用XMLHttpRequest.upload对象的progress事件 ； 对应属性为 onprogress|
| timeout | 当xhr响应超时触发，对应属性为 ontimeout |




参考: https://developer.mozilla.org/en-US/docs/Web/API/XMLHttpRequest
