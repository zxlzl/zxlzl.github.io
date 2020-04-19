---
title: Fetch学习笔记  
date: 2018-02-10 15:21:08  
categories: fetch  
tags:
---

# 什么是Fetch

&emsp;  Fetch 是浏览器提供的原生AJAX接口。由于原来的XMLHttpRequest不符合关注分离原则，且基于事件的模型在处理异步上已经没有现代的Promise等那么有优势。因此Fetch出现来解决这种问题。

&emsp;  [Fetch API ](https://developer.mozilla.org/en-US/docs/Web/API/Fetch_API)提供了能够用于操作一部分 HTTP 的 JavaScript 接口，比如 requests 和 responses。它同时也提供了一个全局的 fetch() 方法——能够简单的异步的获取资源。 

## 1. 简单示例
``` Fetch
fetch("/data.json").then(function(res) {
  // res instanceof Response == true.
  if (res.ok) {
    res.json().then(function(data) {
      console.log(data.entries);
    });
  } else {
    console.log("Looks like the response wasn't perfect, got status", res.status);
  }
}, function(e) {
  console.log("Fetch failed!", e);
});
```
如果是提交一个POST请求，代码如下：
``` post请求
fetch("http://www.example.org/submit.php", {
  method: "POST",
  headers: {
    "Content-Type": "application/x-www-form-urlencoded"
  },
  body: "firstName=Nikhil&favColor=blue&password=easytoguess"
}).then(function(res) {
  if (res.ok) {
    alert("Perfect! Your settings are saved.");
  } else if (res.status == 401) {
    alert("Oops! You are not authorized.");
  }
}, function(e) {
  alert("Error submitting form!");
});
```
`fetch()` 方法的参数和 `Request()` 构造函数的参数完全一致，所以我们可以传任意复杂的参数来实现更强大的 `fetch()`。

## 2. Headers
Fetch 引入了 3 个接口，分别是 `Headers`，`Request` 和 `Response`。他们直接对应于的 HTTP 中相应的概念，但是基于隐私和安全考虑，也有些区别，例如支持 CORS 规则以及保证 cookies 不能被第三方获取。

`Headers` 接口是一个简单的键值对：
``` Headers 接口
var content = "Hello World";
var reqHeaders = new Headers();
reqHeaders.append("Content-Type", "text/plain");
reqHeaders.append("Content-Length", content.length.toString());
reqHeaders.append("X-Custom-Header", "ProcessThisImmediately");
```
也可以给构造函数传一个多维数组或 JS 字面量对象：
```
reqHeaders = new Headers({
  "Content-Type": "text/plain",
  "Content-Length": content.length.toString(),
  "X-Custom-Header": "ProcessThisImmediately",
});
```
Headers 的内容可被检索：
```
console.log(reqHeaders.has("Content-Type")); // true
console.log(reqHeaders.has("Set-Cookie")); // false
reqHeaders.set("Content-Type", "text/html");
reqHeaders.append("X-Custom-Header", "AnotherValue");
 
console.log(reqHeaders.get("Content-Length")); // 11
console.log(reqHeaders.getAll("X-Custom-Header")); // ["ProcessThisImmediately", "AnotherValue"]
 
reqHeaders.delete("X-Custom-Header");
console.log(reqHeaders.getAll("X-Custom-Header")); // []
```
一些操作只在 ServiceWorkers 中可用，但这些 API 使得操作 header 更为方便。

由于 header 可以在发送请求时被发送或在收到响应时被接收，并规定了那些参数可写，所以在 `Headers` 对象中有个一 `guard` 属性，来指定哪些参数可以被改变。

可能的值如下：

 - `"none"`：默认值
 - `"request"`：`Request.headers` 对象只读
 - `"request-no-cors"`：在 `no-cors` 模式下，`Request.headers` 对象只读
 - `"response"`：`Response.headers` 对象只读
 - `"immutable"`：通常在 ServiceWorkers 中使用，所有 Header 对象都为只读

在规范中对每个 `guard` 属性值有更详细的描述。例如，当 `guard` 为 `request` 时，你将不能添加或修改`header 的 Content-Length` 属性。

如果使用了一个不合法的 HTTP Header 名，那么 Headers 的方法通常都抛出 TypeError 异常。如果不小心写入了一个只读属性，也会抛出一个 TypeError 异常。除此以外，失败了将不抛出任何异常。例如：
```
var res = Response.error();
try {
  res.headers.set("Origin", "http://mybank.com");
} catch(e) {
  console.log("Cannot pretend to be a bank!");
}
```

## 3. Request
通过构造一个 `Request` 对象来获取网络资源，构造函数需要 `URL`、`method` 和 `headers` 参数，同时也可以提供请求体（body）、请求模式（mode）、`credentials` 和 `cache hints` 等参数。

最简单的形式如下：
```
var req = new Request("/index.html");
console.log(req.method); // "GET"
console.log(req.url); // "http://example.com/index.html"
```
也可以将一个 `Request` 对象传给构造函数，这将返回该对象的一个副本（这与 `clone()` 方法不同，后面将介绍）。
```
var copy = new Request(req);
console.log(copy.method); // "GET"
console.log(copy.url); // "http://example.com/index.html"
```
同时，这种形式通常只在 ServiceWorkers 中使用。

除 `URL` 之外的参数只能通过第二个参数传递，该参数是一个键值对：
```
var uploadReq = new Request("/uploadImage", {
  method: "POST",
  headers: {
    "Content-Type": "image/png",
  },
  body: "image data"
});
```
`mode` 参数用来决定是否允许跨域请求，以及哪些 `response` 属性可读。可选的 `mode` 值为 `"same-origin"`、`"no-cors"`（默认）以及 `"cors"`。

## 4. same-origin
该模式很简单，如果一个请求是跨域的，那么将返回一个 `error`，这样确保所有的请求遵守同源策略。
```
var arbitraryUrl = document.getElementById("url-input").value;
fetch(arbitraryUrl, { mode: "same-origin" }).then(function(res) {
  console.log("Response succeeded?", res.ok);
}, function(e) {
  console.log("Please enter a same-origin URL!");
});
```

## 5. no-cors
该模式允许来自 CDN 的脚本、其他域的图片和其他一些跨域资源，但是首先有个前提条件，就是请求的 method 只能是`HEAD`、`GET` 或 `POST`。此外，如果 ServiceWorkers 拦截了这些请求，它不能随意添加或者修改除这些之外 Header 属性。第三，JS 不能访问 Response 对象中的任何属性，这确保了跨域时 ServiceWorkers 的安全和隐私信息泄漏问题。
    
## 6.cors
该模式通常用于跨域请求，用来从第三方提供的 API 获取数据。该模式遵守 [CORS 协议](http://www.ruanyifeng.com/blog/2016/04/cors.html)，并只有有限的一些 Header 被暴露给 Response 对象，但是 body 是可读的。例如，获取一个 Flickr 最感兴趣的照片的清单：
```
var u = new URLSearchParams();
u.append('method', 'flickr.interestingness.getList');
u.append('api_key', '<insert api key here>');
u.append('format', 'json');
u.append('nojsoncallback', '1');
 
var apiCall = fetch('https://api.flickr.com/services/rest?' + u);
 
apiCall.then(function(response) {
  return response.json().then(function(json) {
    // photo is a list of photos.
    return json.photos.photo;
  });
}).then(function(photos) {
  photos.forEach(function(photo) {
    console.log(photo.title);
  });
});
```
我们无法从 Headers 中读取 Date 属性，因为 Flickr 在 Access-Control-Expose-Headers 中设置了不允许读取它。
```
response.headers.get("Date"); // null
```

另外，`credentials` 属性决定了是否可以跨域访问 cookie 。该属性与 XHR 的
`withCredentials` 标志相同，但是只有三个值，分别是 `omit`（默认）、`same-origin` 和 `include`。

`Request` 对象也提供了客户端缓存机制（caching hints）。这个属性还在安全复审阶段。Firefox 提供了这个属性，但目前还不起作用。

Request 对象还有两个与 ServiceWorks 拦截有关的只读属性。其中一个是`referrer`，表示该 Request 的来源，可能为空。另外一个是 `context`，是一个非常大的枚举集合，定义了获得的资源的种类，它可能是 image 当请求来自于 img 标签时，可能是 worker 如果是一个 Worker 脚本，等等。如果使用 fetch() 函数，这个值是 fetch。

## 7. Response
Response 对象通常在 `fetch()` 的回调中获得，也可以通过 JS 构造，不过这通常只在 ServiceWorkers 中使用。

Response 对象中最常见的属性是 `status`（整数，默认值是 `200`）和`statusText`（默认值是 `"OK"`）。还有一个 ok 属性，这是 `status` 值为 `200~299` 时的语法糖。

另外，还有一个 `type` 属性，它的值可能是 `"basic"`、`"cors"`、`"default"`、`"error"` 或 `"opaque"`。

 - `"basic"`：同域的响应，除 `Set-Cookie` 和 `Set-Cookie` 之外的所有 Header 可用
 - `"cors"`：Response 从一个合法的跨域请求获得，某些 Header 和 body 可读
 - `"error"`：网络错误。Response 对象的 status 属性为 0，headers 属性为空并且不可写。当 `Response`对象从 `Response.error()` 中得到时，就是这种类型
 - `"opaque"`：在 `"no-cors"` 模式下请求了跨域资源。依靠服务端来做限制

当 `type` 属性值为 `"error"` 时会导致 fetch() 方法的 `Promise` 被 `reject`，reject 回调的参数为 TypeError 对象。

还有一些属性只在 ServerWorker 下有效。在 ServerWorker 下返回一个 Response 的正确方式为：
```
addEventListener('fetch', function(event) {
  event.respondWith(new Response("Response body", {
    headers: { "Content-Type" : "text/plain" }
  });
});
```
正如我们所见，Response 构造函数接收两个参数：返回的 body 和一个键值对对象，通过该对象来设置 `status`、`statusText` 和 `headers` 属性。

静态方法 `Response.error()` 将返回一个错误响应，`Response.redirect(url, status)` 将返回一个跳转响应。



