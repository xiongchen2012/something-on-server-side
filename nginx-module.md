#### Nginx/Tengine安装模块

1. Nginx/Tengine 静态编译模块
 下载模块代码，然后解压：
 <pre>cd /path/to/nginx_sources></pre>
 <pre>./configure --add-module=/path/to/module_sources</pre>
 <pre>make & make install</pre>
这样的话模块就被编到Nginx/Tengine中，可以直接使用

2. Tengine有DSO动态模块加载机制，并提供一个叫dso_tool的工具把模块编译成动态模块
 下载模块代码，然后解压：
 <pre>cd /path/to/tengine/sbin</pre>
 <pre>sudo ./dso_tool --add-module=/path/to/module_sources</pre>

  模块的so文件会被安装到tengine的modules目录下
  <pre>
  dso {
      load ngx_xxxx_module.so;
  }
  </pre>
