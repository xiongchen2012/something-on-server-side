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

