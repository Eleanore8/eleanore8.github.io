---
layout: default
title:  "preload、prefetch、Priorities"
date:   2020-04-12 17:50:00
des:  "对于当前页面很有必要的资源使用 preload，对于可能在将来的页面中使用的资源使用 prefetch。"
motto:  ""
categories: 网络请求
---

preload 是声明式的 fetch，可以强制浏览器请求资源，同时不阻塞文档 onload 事件。

Prefetch 提示浏览器这个资源将来可能需要，但是把决定是否和什么时间加载这个资源的决定权交给浏览器。

![preload]({{ site.baseurl }}/img/preload.png)

#### 缓存

preload 和 prefetch 都被存储在 HTTP 缓存中。

当一个资源被 preload 或者 prefetch 获取后，它可以从 HTTP 缓存移动至渲染器的内存缓存中。
如果资源可以被缓存（比如说存在有效的cache-control 和 max-age），它被存储在 HTTP 缓存中可以被现在或将来的任务使用，
如果资源不能被缓存在 HTTP 缓存中，作为代替，它被放在内存缓存中直到被使用。

#### 警惕二次获取

1. 不要用 “prefetch” 作为 “preload” 的后备，它们适用于不同的场景，常常会导致不符合预期的二次获取。使用 preload 来获取当前需要任务否则使用 prefetch 来获取将来的任务，不要一起用。

2. preload 字体不带 crossorigin 也将会二次获取！ 
确保你对 preload 的字体添加 crossorigin 属性，否则他会被下载两次，这个请求使用匿名的跨域模式。
这个建议也适用于字体文件在相同域名下，也适用于其他域名的获取(比如说默认的异步获取)。



