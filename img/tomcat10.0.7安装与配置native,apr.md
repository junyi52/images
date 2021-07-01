## tomcat10.0.7安装与配置native,apr

1.安装APR : yum -y install apr apr-devel 

![image-20210630152939773](https://i.loli.net/2021/06/30/7cGZsP5wLWp9JBv.png)

2.安装native : 先进入tomcat/bin目录

![image-20210630153113790](https://i.loli.net/2021/06/30/FzdjhECSVb1WQBr.png)

3.解压tomcat-native.tar.gz后进入解压后文件夹下的native目录中执行*`./configure --with-apr=/usr/bin/apr-1-config`*

![image-20210630153334069](https://i.loli.net/2021/06/30/JB1DULwRP4a2HTS.png)
再执行*`make && make install`*

安装完成之后 会出现如下提示信息 
Libraries have been installed in: 

/usr/local/apr/lib

4.vi /etc/profile 修改环境变量，添加 
export LD_LIBRARY_PATH=/usr/local/apr/lib 
source /etc/profile 使得修改生效

5.安装成功后还需要对tomcat设置环境变量，方法是在catalina.sh文件中第二行增加如下内容： 
CATALINA_OPTS='-Djava.library.path=/usr/local/apr/lib' 
记得加在文件前面！

![image-20210630154157604](https://i.loli.net/2021/06/30/QVFW8exPSLqZkOR.png)

6.修改8080端对应的conf/server.xml

> protocol="org.apache.coyote.http11.Http11AprProtocol"
>
> 添加enableLookups=”false”来关闭DNS反向查询提升性能。

![image-20210630154325040](https://i.loli.net/2021/06/30/bjXLqfw2CkU9vgS.png)

server.xml有如下一项默认设置

> <Listener className="org.apache.catalina.core.AprLifecycleListener" SSLEngine="on" />
>
> 如果用不到SSL，则需要关闭，on改为off,否则启动时会报错

![image-20210630154518142](https://i.loli.net/2021/06/30/nmJ46kjRqxvVtyw.png)

启动tomcat之后，会看到如下的一条日志，说明APR模式启动成功

> INFO: APR capabilities: IPv6 [true], sendfile [true], accept filters [false], random [true].

![image-20210630154819851](https://i.loli.net/2021/06/30/Q1sWHZ7cLBhOFJ9.png)