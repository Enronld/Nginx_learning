# Nginx

## 1.nginx基本概念

### 1.1 nginx是什么

高性能的HTTP和反向代理服务器

### 1.2 反向代理

1. **正向代理**

   在客户端（浏览器）配置代理服务器，通过代理服务器进行互联网访问

   ![1618840343973](https://raw.githubusercontent.com/Enronld/Nginx_learning/main/images/1618840343973.png)

2. **反向代理**

   

   ![1618840295686](https://raw.githubusercontent.com/Enronld/Nginx_learning/main/images/1618840295686.png)

### 1.3 负载均衡

增加服务器的数量，然后将请求分发到各个服务器上，将原先请求集中到单个服务器上的情况改为将请求分发到多个服务器上，将负载分发到不同的服务器

![1618841320337](https://raw.githubusercontent.com/Enronld/Nginx_learning/main/images/1618841320337.png)



### 1.4 动静分离

把动态页面和静态页面由不同的服务器来解析，加快解析速度，降低原来单个服务器的压力。

![1618842135776](https://raw.githubusercontent.com/Enronld/Nginx_learning/main/images/1618842135776.png)



## 2.nginx安装、常用命令和配置文件

### 2.1 在linux系统中安装nginx

1. 使用远程工具连接Linux操作系统

![1618927884695](https://raw.githubusercontent.com/Enronld/Nginx_learning/main/images/1618927884695.png)

**需要：pcre-8.37.tar.gz | openssl-1.0.1t.tar.gz | zlib-1.2.8.tar.gz | nginx-1.11.1.tar.gz**

​    2. 安装pcre依赖

- 把安装压缩文件放到linux系统中

  ![1618928761877](https://raw.githubusercontent.com/Enronld/Nginx_learning/main/images/1618928761877.png)

- 解压压缩文件

  ![1618928978357](https://raw.githubusercontent.com/Enronld/Nginx_learning/main/images/1618928978357.png)

- 进入解压之后的目录，执行./configure

  ![1618929194948](https://raw.githubusercontent.com/Enronld/Nginx_learning/main/images/1618929194948.png)



- 使用make && make install

  ![1618929271879](https://raw.githubusercontent.com/Enronld/Nginx_learning/main/images/1618929271879.png)

- 使用命令查看版本号

![1618935880904](https://raw.githubusercontent.com/Enronld/Nginx_learning/main/images/1618935880904.png)

3. 安装其它依赖

   yum -y install make zlib zlib-devel gcc-c++ libtool openssl openssl-devel

4. 安装nginx

   - 把ngnix安装文件放到linux系统中

   - 解压压缩文件

   - 进入解压目录，执行./configure

   - 使用make && make install

     ![1618936526206](https://raw.githubusercontent.com/Enronld/Nginx_learning/main/images/1618936526206.png)

![1618937468664](https://raw.githubusercontent.com/Enronld/Nginx_learning/main/images/1618937468664.png)

![1618937515461](C:\Users\a\AppData\Roaming\Typora\typora-user-images\1618937515461.png)

![1618937666623](https://raw.githubusercontent.com/Enronld/Nginx_learning/main/images/1618937666623.png)

![1618937713849](https://raw.githubusercontent.com/Enronld/Nginx_learning/main/images/1618937713849.png)



查看开放端口号

firewall-cmd --list-all(centos7)

==service iptables status(centos6)==

![1618938767282](https://raw.githubusercontent.com/Enronld/Nginx_learning/main/images/1618938767282.png)

设置开放端口号

firewall-cmd --add-service=http -permanebt(centos7)

sudo firewall-cmd --add-port=80/tcp --permanent(centos7)

==/sbin/iptables -I INPUT -p tcp --dport 8001 -j ACCEPT(centos6)==

重启防火墙

firewall-cmd -reload(centos7)

==/etc/init.d/iptables save (centos6)==

![1618938752593](https://raw.githubusercontent.com/Enronld/Nginx_learning/main/images/1618938752593.png)



**注意**

**YumRepo Error: All mirror URLs are not using ftp, http[s] or file.**

修改Centos-base.repo为阿里镜像

[base]
name=CentOS-$releasever - Base - mirrors.aliyun.com
failovermethod=priority
baseurl=http://mirrors.aliyun.com/centos-vault/centos/$releasever/os/$basearch/
        http://mirrors.aliyuncs.com/centos-vault/centos/$releasever/os/$basearch/
        http://mirrors.cloud.aliyuncs.com/centos-vault/centos/$releasever/os/$basearch/
gpgcheck=1
gpgkey=http://mirrors.aliyun.com/centos/RPM-GPG-KEY-CentOS-6

#released updates 
[updates]
name=CentOS-$releasever - Updates - mirrors.aliyun.com
failovermethod=priority
baseurl=http://mirrors.aliyun.com/centos-vault/centos/$releasever/updates/$basearch/
        http://mirrors.aliyuncs.com/centos-vault/centos/$releasever/updates/$basearch/
        http://mirrors.cloud.aliyuncs.com/centos-vault/centos/$releasever/updates/$basearch/
gpgcheck=1
gpgkey=http://mirrors.aliyun.com/centos/RPM-GPG-KEY-CentOS-6

#additional packages that may be useful
[extras]
name=CentOS-$releasever - Extras - mirrors.aliyun.com
failovermethod=priority
baseurl=http://mirrors.aliyun.com/centos-vault/centos/$releasever/extras/$basearch/
        http://mirrors.aliyuncs.com/centos-vault/centos/$releasever/extras/$basearch/
        http://mirrors.cloud.aliyuncs.com/centos-vault/centos/$releasever/extras/$basearch/
gpgcheck=1
gpgkey=http://mirrors.aliyun.com/centos/RPM-GPG-KEY-CentOS-6

#additional packages that extend functionality of existing packages
[centosplus]
name=CentOS-$releasever - Plus - mirrors.aliyun.com
failovermethod=priority
baseurl=http://mirrors.aliyun.com/centos-vault/centos/$releasever/centosplus/$basearch/
        http://mirrors.aliyuncs.com/centos-vault/centos/$releasever/centosplus/$basearch/
        http://mirrors.cloud.aliyuncs.com/centos-vault/centos/$releasever/centosplus/$basearch/
gpgcheck=1
enabled=0
gpgkey=http://mirrors.aliyun.com/centos/RPM-GPG-KEY-CentOS-6

#contrib - packages by Centos Users
[contrib]
name=CentOS-$releasever - Contrib - mirrors.aliyun.com
failovermethod=priority
baseurl=http://mirrors.aliyun.com/centos-vault/centos/$releasever/contrib/$basearch/
        http://mirrors.aliyuncs.com/centos-vault/centos/$releasever/contrib/$basearch/
        http://mirrors.cloud.aliyuncs.com/centos-vault/centos/$releasever/contrib/$basearch/
gpgcheck=1
enabled=0
gpgkey=http://mirrors.aliyun.com/centos/RPM-GPG-KEY-CentOS-6

### 2.2 nginx常用命令

1. 使用nginx操作命令前提条件：必须进入nginx目录 /usr/local/nginx/sbin

2. 查看nginx版本号

   **./nginx -v**

   ![1619187225248](https://raw.githubusercontent.com/Enronld/Nginx_learning/main/images/1619187225248.png)

3. 启动nginx

   **./nginx**

   ![1619187455362](https://raw.githubusercontent.com/Enronld/Nginx_learning/main/images/1619187455362.png)

   

4. 关闭nginx

   **./nginx -s stop**

   ![1619187486847](https://raw.githubusercontent.com/Enronld/Nginx_learning/main/images/1619187486847.png)

   

5. 重新加载nginx

   **./nginx -s reload**

   ![1619187578471](https://raw.githubusercontent.com/Enronld/Nginx_learning/main/images/1619187578471.png)

### 2.3 nginx配置文件

1. nginx配置文件位置

   /usr/local/nginx/conf/nginx.conf

![1619187709694](https://raw.githubusercontent.com/Enronld/Nginx_learning/main/images/1619187709694.png)

2. nginx配置文件组成

    **nginx配置文件有三部分组成**

   - 全局块

     从配置文件开始到events块之间的内容，主要会设置一些影响nginx服务器整体运行的配置指令

     ![1619188800992](https://raw.githubusercontent.com/Enronld/Nginx_learning/main/images/1619188800992.png)

   - events块

     影响nginx服务器与用户的网络连接

     ![1619188943321](https://raw.githubusercontent.com/Enronld/Nginx_learning/main/images/1619188943321.png)

   - http块

     nginx服务器配置中最频繁的部分

     ①http全局快

     文件引入、MIME-TYPE定义、日志自定义、连接超时时间、单链接请求数上限

     ![1619189117247](https://raw.githubusercontent.com/Enronld/Nginx_learning/main/images/1619189117247.png)

     ②server块

     全局server块\location块

     ![1619189187588](https://raw.githubusercontent.com/Enronld/Nginx_learning/main/images/1619189187588.png)

     

## 3.nginx配置实例 1-反向代理实例1

1. 实现效果

   打开浏览器，在浏览器地址栏输入地址www.123.com，跳转linux系统tomcat主页面中

2. 准备工作

   - 在linux系统安装tomcat,使用默认端口8080

     tomcat安装文件放到linux系统中，解压

     进入tomcat的bin目录中，./startup.sh启动tomcat服务器

   ![1619190907967](https://raw.githubusercontent.com/Enronld/Nginx_learning/main/images/1619190907967.png)

   ![1619190959095](https://raw.githubusercontent.com/Enronld/Nginx_learning/main/images/1619190959095.png)

   - 对外开放访问端口

     ![1619191259791](https://raw.githubusercontent.com/Enronld/Nginx_learning/main/images/1619191259791.png)

3. 在windows系统中通过浏览器访问tomcat服务器

   ![1619191369135](https://raw.githubusercontent.com/Enronld/Nginx_learning/main/images/1619191369135.png)

4. 访问过程的分析

   ![1619191695525](https://raw.githubusercontent.com/Enronld/Nginx_learning/main/images/1619191695525.png)

5. 具体配置

   ①在windows系统的Host文件中进行域名和ip对应关系的配置

   ![1619191823711](https://raw.githubusercontent.com/Enronld/Nginx_learning/main/images/1619191823711.png)

   ![1619191891858](C:\Users\a\AppData\Roaming\Typora\typora-user-images\1619191891858.png)

   ②在nginx进行请求转发的配置(反向代理配置)

   ![1619192140973](https://raw.githubusercontent.com/Enronld/Nginx_learning/main/images/1619192140973.png)

6. 最终测试

   ![1619192929248](https://raw.githubusercontent.com/Enronld/Nginx_learning/main/images/1619192929248.png)

## 4.nginx配置实例 1-反向代理实例2

1. 实现效果

   使用nginx反向代理，根据访问的路径跳转到不同端口的服务中

   nginx监听端口为9001

   访问http://127.0.0.1:9001/edu/ 直接跳转到 127.0.0.1:8081

   访问http://127.0.0.1:9001/vod/ 直接跳转到 127.0.0.1:8082

2. 准备工作

   ①准备两个tomcat服务器，一个8080端口，一个8081端口

   修改8081tomcat端口 conf -> server.xml

   ![1619193784234](https://raw.githubusercontent.com/Enronld/Nginx_learning/main/images/1619193784234.png)

   ![1619193803434](https://raw.githubusercontent.com/Enronld/Nginx_learning/main/images/1619193803434.png)

   ![1619193929210](https://raw.githubusercontent.com/Enronld/Nginx_learning/main/images/1619193929210.png)

   如果是自己配的jdk tomcat可参照https://blog.csdn.net/chenshiyang0806/article/details/81407766

   **有问题一定学会要看日志 不然找不出错误**

   典型问题

   ![1619198992743](https://raw.githubusercontent.com/Enronld/Nginx_learning/main/images/1619198992743.png)

![1619199130035](https://raw.githubusercontent.com/Enronld/Nginx_learning/main/images/1619199130035.png)

![1619199152460](https://raw.githubusercontent.com/Enronld/Nginx_learning/main/images/1619199152460.png)

②创建文件夹和测试页面

![1619199470060](https://raw.githubusercontent.com/Enronld/Nginx_learning/main/images/1619199470060.png)

![1619199448309](https://raw.githubusercontent.com/Enronld/Nginx_learning/main/images/1619199448309.png)

③具体配置

找到nginx配置文件，进行反向代理设置

~*不区分大小写

=严格匹配

![1619199762914](https://raw.githubusercontent.com/Enronld/Nginx_learning/main/images/1619199762914.png)

④开放对外端口号 9001 8081 8080

![1619199868166](https://raw.githubusercontent.com/Enronld/Nginx_learning/main/images/1619199868166.png)

⑤效果

![1619200118558](https://raw.githubusercontent.com/Enronld/Nginx_learning/main/images/1619200118558.png)

![1619200130502](https://raw.githubusercontent.com/Enronld/Nginx_learning/main/images/1619200130502.png)

## 5.nginx配置实例 2-负载均衡

1. 实现效果

   - 浏览器地址栏输入地址http://192.168.153.10/edu/a.html,负载均衡效果，平均8080和8081端口中

2. 准备工作

   ①准备两台tomcat服务器，一台8080，一台8081

   ②在两台tomcat里面webapps目录中，创建名称是edu文件夹，在edu文件夹中创建页面a.html，用于测试

3. 在nginx的配置文件中进行负载均衡的配置

   ![1619276356503](https://raw.githubusercontent.com/Enronld/Nginx_learning/main/images/1619276356503.png)

![1619276389820](C:\Users\a\AppData\Roaming\Typora\typora-user-images\1619276389820.png)

4. 分配策略

   ①轮询(默认)

   每个请求按照时间顺序逐一分配到不同的后端服务器，如果后端服务器down掉，能自动剔除。

   ②weight

   weight代表权重默认为1，权重越高被分配的客户端越多

   ![1619277123362](https://raw.githubusercontent.com/Enronld/Nginx_learning/main/images/1619277123362.png)

   ③ip_hash

   每个请求按访问ip的hash结果分配，这样每个访客固定访问一个后端服务器，可以解决session的问题。

![1619277293099](https://raw.githubusercontent.com/Enronld/Nginx_learning/main/images/1619277293099.png)

​	④fair

​	按后端服务器的响应时间来分配请求，响应时间短的优先分配

![1619277425163](https://raw.githubusercontent.com/Enronld/Nginx_learning/main/images/1619277425163.png)

## 6.nginx配置实例 3-动静分离

1. 什么是动静分离

   ![1619364821900](https://raw.githubusercontent.com/Enronld/Nginx_learning/main/images/1619364821900.png)



实现大致分为两种：

一种是纯粹把静态文件独立成单独的域名，放在独立的服务器上，也是目前主流推崇方案

另一种方法是动态和静态文件混合在一起发布，通过nginx来分开

- location:指定不同的后缀名实现不同的请求转发

- expires:使浏览器缓存过期时间，减少与服务器之间的请求与流量

  例：设置3d,表示3天之内访问这个url，发送一个请求，比对服务器该文件最后更新时间没有变化，则不会从服务器抓取，返回状态码304；如果有修改，则直接从服务器重新下载，返回状态码200。

2. 准备工作

   在linux系统中准备静态资源，用于进行访问

   ![1619366336470](https://raw.githubusercontent.com/Enronld/Nginx_learning/main/images/1619366336470.png)

3. 具体配置

   ①在nginx配置文件中进行

   ![1619366882043](https://raw.githubusercontent.com/Enronld/Nginx_learning/main/images/1619366882043.png)

4. 最终测试

   ①浏览器中输入地址

   http://192.168.153.10/image/01.jpg

   ![1619367035186](https://raw.githubusercontent.com/Enronld/Nginx_learning/main/images/1619367035186.png)

   ![1619367053302](https://raw.githubusercontent.com/Enronld/Nginx_learning/main/images/1619367053302.png)

   ②在浏览器输入地址

   http://192.168.153.10/www/a.html

   ![1619367114317](https://raw.githubusercontent.com/Enronld/Nginx_learning/main/images/1619367114317.png)

## 7.nginx配置高可用集群(环境限制)

1. 什么是nginx高可用

   ![1619367972734](https://raw.githubusercontent.com/Enronld/Nginx_learning/main/images/1619367972734.png)

   (1)需要两台nginx服务器

   (2)需要keepalived

   (3)需要虚拟Ip

2. 配置高可用的准备工作

## 8.nginx原理

1. master&worker

![1620283944578](https://raw.githubusercontent.com/Enronld/Nginx_learning/main/images/1620283944578.png)

2. worker怎么工作？

   争抢机制

3. 一个master、多个worker好处

   - 可以使用 nginx -s reload热部署

   - 每个worker都是独立的进程，不需要加锁；一个进程退出，其它进程还在工作，服务不会中断

4. 设置多少个worker

   worker数和服务器的cpu数相等是最为适宜的

5. worker_connection worker连接数

   - 发送请求，占用了worker的几个连接数？

     静态资源2个      tomcat访问数据库2个     一共4个

   - nginx有一个master,有4个worker,每个worker支持最大连接数据1024，支持的最大并发数是多少？

     worker最大支持的连接数 4*1024

     最大并发：4✖1024÷2    or   4✖1024÷4

## 9.静态文件服务器案例demo

### 9.1 centos安装ftp服务器

1. ​	安装vsftpd

   ​	![1619538775664](https://raw.githubusercontent.com/Enronld/Nginx_learning/main/images/1619538775664.png)

2. ​	关闭匿名访问

   ![1619538880571](https://raw.githubusercontent.com/Enronld/Nginx_learning/main/images/1619538880571.png)

   ​	![1619538864626](https://raw.githubusercontent.com/Enronld/Nginx_learning/main/images/1619538864626.png)

3. ​	启动服务

   ​	![1619539105044](https://raw.githubusercontent.com/Enronld/Nginx_learning/main/images/1619539105044.png)

4. ​	查看服务状态

   ​	![1619539139430](https://raw.githubusercontent.com/Enronld/Nginx_learning/main/images/1619539139430.png)

5. ​	开放访问端口

   ​	![1619539876550](https://raw.githubusercontent.com/Enronld/Nginx_learning/main/images/1619539876550.png)

6. 设置登录用户名密码

   ![1619540154379](https://raw.githubusercontent.com/Enronld/Nginx_learning/main/images/1619540154379.png)



![1619540175845](https://raw.githubusercontent.com/Enronld/Nginx_learning/main/images/1619540175845.png)

![1619583464203](https://raw.githubusercontent.com/Enronld/Nginx_learning/main/images/1619583464203.png)

​		![1619587776398](https://raw.githubusercontent.com/Enronld/Nginx_learning/main/images/1619587776398.png)

### 9.2 springboot上传文件到ftp服务器

1. 添加依赖

![1620270924458](https://raw.githubusercontent.com/Enronld/Nginx_learning/main/images/1620270924458.png)

2. 封装文件上传工具类

   ```
   public class FtpUtil {
   
       //ftp服务器ip地址
       private static final String FTP_ADDRESS = "192.168.153.10";
       //端口号
       private static final int FTP_PORT = 21;
       //用户名
       private static final String FTP_USERNAME = "enron";
       //密码
       private static final String FTP_PASSWORD = "yxfld";
       //附件路径
       private static final String FTP_BASEPATH = "/home/enron/files";
   
       public static String uploadFile(MultipartFile file) throws IOException {
   
           //获取上传的文件流
           InputStream inputStream = file.getInputStream();
           //获取上传文件名
           String filename = file.getOriginalFilename();
           //截取后缀
           String suffix = filename.substring(filename.lastIndexOf("."));
           //使用UUID拼接后缀，定义一个不重复的文件名
           String finalName = UUID.randomUUID() + suffix;
   
           FTPClient ftp = new FTPClient();
           try{
               int reply;
               ftp.connect(FTP_ADDRESS,FTP_PORT);  //连接ftp服务器
               ftp.login(FTP_USERNAME,FTP_PASSWORD);   //登录
               reply = ftp.getReplyCode();
               if (!FTPReply.isPositiveCompletion(reply)){
                   ftp.disconnect();
                   return null;
               }
               ftp.setFileType(FTPClient.BINARY_FILE_TYPE);
               ftp.makeDirectory((FTP_BASEPATH));
               ftp.changeWorkingDirectory(FTP_BASEPATH);
               ftp.enterLocalPassiveMode();
               ftp.storeFile(finalName,inputStream);
               inputStream.close();
               ftp.logout();
   
           } catch (IOException e){
               e.printStackTrace();
               return null;
           } finally {
               if (ftp.isConnected()){
                   try{
                       ftp.disconnect();
                   } catch (IOException ioe){
                       ioe.printStackTrace();
                   }
               }
           }
           return finalName;
       }
   
   
   }
   ```

3. 配置nginx映射 nginx.conf

   ![1620276505281](https://raw.githubusercontent.com/Enronld/Nginx_learning/main/images/1620276505281.png)

   ![1620276552823](https://raw.githubusercontent.com/Enronld/Nginx_learning/main/images/1620276552823.png)

   > 权限不够 只有enron用户可访问 需要添加权限
   >
   > 

   ![1620276653936](https://raw.githubusercontent.com/Enronld/Nginx_learning/main/images/1620276653936.png)

4. 访问成功

   ![1620276682119](https://raw.githubusercontent.com/Enronld/Nginx_learning/main/images/1620276682119.png)

5. 在controller里实现前后端交互文件上传

   ```
   //接收图片
   @RequestMapping(value = "uploadImage", method = RequestMethod.POST)
   @ResponseBody
   public String uploadImage(MultipartFile file){
       
       try {
           //调用自定义的FTP工具类上传文件
           String fileName = FtpUtil.uploadFile(file);
           return "http://192.168.153.10/files/" +  fileName;
       }catch (Exception e) {
           e.printStackTrace();
       }
       return null;
   
   }
   ```

6. 前后端连接查看文件上传效果

   ![1620276990416](https://raw.githubusercontent.com/Enronld/Nginx_learning/main/images/1620276990416.png)

   ![1620276973143](https://raw.githubusercontent.com/Enronld/Nginx_learning/main/images/1620276973143.png)

