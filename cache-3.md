## 浏览器缓存（HTTP缓存）

和ApplicationCache相似，HTTP缓存的目标也是为了让浏览器使用本地缓存，而不去向服务器请求。

浏览器通过和服务器事先的约定来决定是否使用保存在浏览器本地的缓存，这个约定就是[HTTP协议](https://zh.wikipedia.org/wiki/%E8%B6%85%E6%96%87%E6%9C%AC%E4%BC%A0%E8%BE%93%E5%8D%8F%E8%AE%AE "HTTP协议")。协议里规定Client和Server协商使用缓存通过协议头（Header）来进行的，也就是传说中的 [夹带私货](https://www.w3.org/Protocols/rfc2616/rfc2616-sec13.html "HTTP Cache Headers")

有多种种Http Cache Headers

### Expires

服务器在返回给浏览器的响应头（Response Header）中夹带返回访资源过期的时间，比如：
```
Expires: Sat, 2 Feb 2018 23:59:59 GMT
```

翻译成人话：当前这一资源将于2018年2月3日 23时59分59秒过期。在这之前浏览器您可以放心使用本地缓存，过了这个时间您就需要再向服务器请求了。

适用于：`HTTP1.0`、`HTTP1.1`

**缺点：**缺点很明显，服务器时间不一定和客户端时间一致啊！！

### Cache-Control

为了解决Expires的问题，引进了Cache-Control，服务器返回一个相对的秒数，表示从这个资源下载完成的时间算起，N秒内都可以放心将其放到缓存中使用。N秒后浏览器才需要重新下载。
```
Cache-Control: max-age=3600
```

翻译成人话：当前页面有效期为3600秒，这段时间内你可以直接使用这个页面无需重新下载。过了3600秒后，你就需要重新向服务器请求了。

适用于：`HTTP1.1`

### Last-Modified / If-Modified-Since

浏览器在第1次请求资源时，如果服务器返回200，响应头里面一般会有一个Last-Modified字段，字段的值服务器最后一次修改此资源的时间。
```
Last-Modified: Sat, 2 Feb 2018 18:39:13 GMT
```

当浏览器再次请求该资源时，会在请求头里夹带一个If-Modified-Since的私货，内容就是第1次服务器返回的那个最后修改时间。这个过程是浏览器向服务器询问当前请求的资源自从最后一次修改以后是否又进行过修改？ 
```
If-Modified-Since: Sat, 2 Feb 2018 18:39:13 GMT
```

服务器收到请后，发现这段时间内没有再修改该资源，就返回状态码[304（Not Modified）](https://httpstatuses.com/304 "304")，请示我没有修改过该资源，浏览器你可以使用你的本地缓存。

### ETag / If-None-Match

HTTP协议对Etag的定义是：[RFC](https://www.w3.org/Protocols/rfc2616/rfc2616-sec14.html#sec14.19)
> The ETag response-header field provides the current value of the entity tag for the requested variant

翻译再加工一下：ETag是客户端所请求的资源的实体的Tag值。所谓实体应该指的是请求URL对应的资源实体（比如1张图片，1个js文件等），实体的Tag值应该指的是可以与实体对应的某个固定的值。（比如文件的MD5值，一旦实体变了，MD5值也必定会跟着变）

我个人理解的ETag值：服务器为返回给客户端（浏览器）的资源实体打上的标签，是为了方便与客户端协商是否使用缓存的关键点。比如我给一个图片打了一个Etag值是d41d8c，第一次浏览器请求这个图片时将Etag存下来。第二次浏览器请求这个图片前，先把这个值（d41d8c）传给服务器，服务器比较传过来的这个Etag值和当前值是不是一致，如果一致就直接返回304。否则返200。这样浏览器就知道是放心使用本地缓存还是应该重新请求新的图片了。

客户端发送Etag向服务端查询资源是否更新的过程，有个专门的术语，叫Conditional GET（带条件的GET请求），这是一个代价很小的HTTP请求。

整个过程如下：

1. 第一次浏览器向服务端请求资源，服务端在响应的`body`发送该资源,同时在`header`中附加该资源的`ETag值`<br/>
2. 后面浏览器再用到该资源时，先向服务端发送ConditionalGET请求，在`header`中附件`If-None-Match`字段，该字段的值是第1步中的Etag值。<br/>
3. 服务端检查Etag值，如果资源没有变化，直接返回`304`,同时响应的`body`置空；如果资源发生了变化，则在在响应的`body`发送该资源,同时在`header`中附加新的`ETag值`。<br/>

### 总结：

- 为了减轻服务器压力，尽量使用本地缓存，人们想了N多办法：<br/>
  1.`Cache-Control`（请求前）<br/>
  2.`Expires`（请求前）<br/>
  3.`If-None-Match`、`Etag`（请求服务器）<br/>
  4.`If-Modified-Since`、`Last-Modified`（请求服务器）<br/>

- HTTP协议规定`Etag`、`Last-Modified`同时存在时，必须同时发送给服务器。

- Ctrl + F5强制刷新时，以上规则都不生效。

- 开发人员使用Chrome时，最好Disable Cache。
