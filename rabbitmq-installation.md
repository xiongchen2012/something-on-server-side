## RabbitMQ简易安装与配置

* 下载Erlang和Rabbit MQ的安装包
* 安装依赖的库
  <pre> 
     sudo apt-get install libwxbase2.8-0
     sudo apt-get install libwxgtk2.8-0 
     sudo apt-get install socat</pre>

* 安装Erlang和RabbitMQ
  <pre>
    sudo dpkg -i esl-erlang_19.0-1-ubuntu-trusty_amd64.deb
    sudo dpkg -i rabbitmq-server_3.6.4-1_all.deb</pre>

* 添加用户
  <pre>
    sudo rabbitmqctl add_user root '123qwe!@#'
    sudo rabbitmqctl set_user_tags root administrator</pre>
    
* 开启Web管理
  <pre>
     sudo rabbitmq-plugins enable rabbitmq_management</pre>

* 访问地址：
 http://server-name:15672/


