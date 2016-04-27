
#### 服务器环境配置
1. 上传必须的库到服务器
 <pre>scp /path/to/libssl1.0.0_1.0.2g-1ubuntu1_amd64.deb user@host:/home/user</pre>
 <pre>scp /path/to/openssl_1.0.2g-1ubuntu1_amd64.deb user@host:/home/user</pre>
 <pre>scp /path/to/libssl-dev_1.0.2g-1ubuntu1_amd64.deb user@host:/home/user</pre>

2. 安装ＴＯＭＣＡＴ并配置ＡＰＲ连接器
<pre>安装APR：
scp /path/to/apr-1.5.2.tar.gz user@host:/home/user
tar zxvf apr-1.5.2.tar.gz
./configure
make
make install
</pre>

 <pre>安装APR-Util：
 scp /path/to/apr-util-1.5.4.tar.gz user@host:/home/user
 tar zxvf apr-util-1.5.4.tar.gz
 ./configure --with-apr=/usr/local/apr
 make
 make install
 </pre>


  <pre>安装TOMCAT APR：
  cp /opt/apache-tomcat-8.0.32/bin/tomcat-native.tar.gz /usr/local/src
  tar zxvf tomcat-native.tar.gz
  ./configure --with-apr=/usr/local/apr --with-java-home=/opt/jdk1.8 --with-ssl=/usr/bin
  make
  make install
  </pre>

3. 配置环境变量
   <pre>
   vi /etc/profile
   export JAVA_HOME=/opt/jdk1.8
   export JRE_HOME=/opt/jdk1.8/jre
   export PATH=$PATH:/opt/jdk1.8/bin:/opt/jdk1.8/jre/bin
   export LD_LIBRARY_PATH=/usr/local/apr/lib
   source /etc/profile
   </pre>
