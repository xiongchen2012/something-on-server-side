## HTML5 Application Cache

H5开始引入了[Application Cache](https://html.spec.whatwg.org/#applicationcache "Application Cache")技术，可以在客户端缓存服务器上的HTML页面。基于AppCache可以实现网站的离线访问。

Application Cache的引入，带来几个好处：  

1. 离线访问
2. 由于缓存了页面，访问速度快
3. 减少服务端负载

**Deathdealer注：** 
> Application Cache现在已经是一个已经从Web标准中删除了的过期规范，  
> 虽然主流浏览器都还支持，但是不排除哪天会停止支持，所以尽量不要再使用。  
> 强列建议使用 [Service Worker](https://developer.mozilla.org/zh-CN/docs/Web/API/Service_Worker_API/Using_Service_Workers) 来替代AppCache

### 服务端设置

Application Cache是通过浏览器下载服务器上的manifest清单文件，由mainfest清单文件指定需要缓存的文件列表。按照H5规范，这个文件的MIME头必须是：`text/cache-manifest`，所以服务器端需要设置这个类型的响应头。以最常使用的三种服务器为例：

- Nginx  
  
  修改Nginx配置目录下的`mime.types`文件，添加manifest的MIME：
  ``` shell
   # /path/to/nginx/conf/mime.types
   text/cache-manifest     manifest;
  ```

- Apache
 
  修改Apache目录中的配置文件`.htaccess`文件，添加manifest的MIME:
  ``` shell
   # /path/to/apache2/conf/.htaccess
   AddType text/cache-manifest  .apache
  ```

- Tomcat

  修改Tomcat目录中的配置文件`web.xml`文件，添加manifest的MIME:
  ``` xml
   <!-- /path/to/tomcat/conf/web.xml -->
   <mime-mapping>
       <extention>list</extention>
       <mime-type>text/cache-manifest<mime-type>
   </mime-mapping>
  ```

### 缓存清单文件
1. 在HTML页面的`<html>`增加manifest属性。

``` html
<html manifest='example.appcache'>
  ...
</html>
```

每个指定了 manifest 的页面在用户对其访问时都会被缓存。如果未指定 manifest 属性，则页面不会被缓存（除非在 manifest 文件中直接指定了该页面）。

标注了此属性的HTML文件，可以作为主记录文件，作为application cache的入口（entry）

2. 缓存清单文件（建议扩展名为.appcache）

manifest文件告知浏览器被缓存的内容（以及不缓存的内容），有三个小节：
- ACHE MANIFEST：列出的文件将在首次下载后进行缓存
- NETWORK：列出的文件需要与服务器的连接，并且不缓存
- FALLBACK：列出的文件规定当页面无法访问时的备胎页面（比如 404）

因为AppCache已经不再是Web标准，所以介绍的比较简单。

总结一下吧：
- 随便找一个HTML文件当成入口主记录文件，比如index.html。在index.html的`<html>`加上manifest属性，值指向服务器上的缓存清单文件。
- 缓存清单文件中把要缓存的所有文件在CACHE小节中全列出来。
- 不把需要缓存的文件在NETWORK小节中全列出来。
- 最后再指定一定备胎页面（类似404页面）应急。

（TODO：以后有空再写一下ServiceWorker相关的博客）
