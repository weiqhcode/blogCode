---
title: Nginx编译安装
date: 2022-03-20 10:57:42
tags: 
- Nginx
- vps
categories:
- vps
- Nginx

---

# 检查安装环境

- 我们在安装不同的工具软件的时候，需要安装插件的环境关系

  ​		检查系统版本：`cat /etc/redhat-release`

  ​		查看是否已安装`wget: rpm -qa|wget`

  ​		(注： Linux系统中的wget是一个下载文件的工具)

- ​	否则（结果显示空白）安装：`yum -y install wget`

  ​		安装g编译器：默认的云服务器都是会安装的。

  ​		查看是否已安装编译器：`rpm -qa|gcc`

  ​		否则（结果显示空白）安装：`yum -y install gcc gcc-c++`

- 为什么要装gcc编译器？

  ​	gcc是c语言编译器
  ​	而我们的nginx的编码语言就是c语言

  ​	使用gcc编译器可以编译语言代码为可执行程序
  
# 安装 nginx依赖

1. nginx的Rewrite模块和HTTP核心模块会使用到PRE正则表达式语法

   yum -y install pcre pcre-devel

2. nginx的各种模块中需要使用gzip压缩：

   ```shell
   yum -y install zlib zlib-devel
   ```

   

3. 安全套接字层密码库

   ```shell
   yum -y install openssl openssI-devel
   ```
   
   ## 全部依赖：

```shell
yum install gcc gcc-c++ make unzip pcre pcre-devel zlib zlib-devel libxml2 libxml2-devel  readline readline-devel ncurses ncurses-devel perl-devel perl-ExtUtils-Embed openssl-devel -y
```

# 安装nginx

1. 下载 nginx

​	wget http://nginx.org/download/nginx-1.14.0.tar.gz 或者下载好直接上传

​	解压：`tar-zxvf nginx-1.14.0.tar.gz`

2. 解压nginx

```
cd nginx-1.14.0
```

![20220320122137.png](https://github.com/weiqh2000/blogImages/blob/main/20220320122137.png?raw=true)



```shell
./configure --prefix=/usr/local/src/nginx --with-http_ssl_module --with-http_stub_status_module --with-pcre --with-http_spdy_module
```

```shell
参数说明：
编译选项官方提供的有：
–prefix=path 定义一个目录来保存你的nginx的提供功能的文件夹，就这好比我们安装软件的时候软件存放的目录，如果我们在编译的不指定安装位置，那么默认的位置/usr/local/nginx 目录
–sbin-path=path 设置nginx执行脚本的位置，这里如果设置在path变量里面，就可以在bash环境下，任意使用nginx命令，默认位置prefix/sbin/nginx 注意这里的prefix是
在配置文件里面配置的路径
–conf-path=path 配置nginx配置文件的路径，如果不指定这个选项，那么配置文件的默认路径就会是 prefix/conf/nginx.conf
–pid-path =path 配置nginx.pid file的路径，一般来说，进程在运行的时候的时候有一个进程id，这个id会保存在pid file里面，默认的pid file的放置位置是prefix/logs/nginx.pid
–error-log-path=path 设置错误日志的存放路径，如果不指定，就默认 prefix/logs/error.log
–http-log-path= path 设置http访问日志的路径，如果不指定，就默认 prefix/logs/access.log
–user=name 设置默认启动进程的用户，如果不指定，就默认 nobody
–group=name 设置这个用户所在的用户组，如果不指定，依然是nobody
这些是我们常用的编译选项，其他的可以均保持默认，如需特殊指定，可上nginx官网查阅 http://nginx.org/en/docs/configure.html
下面是一些不常用的选项
–with-http_ssl_module -开启HTTP SSL模块，使NGINX可以支持HTTPS请求。需要安装了OPENSSL
–with-http_flv_module
–with-http_stub_status_module - 启用 “server status” 页(可有可无)
–without-http_gzip_module - 禁用 ngx_http_gzip_module. 如果启用，需要 zlib 。
–without-http_ssi_module - 禁用 ngx_http_ssi_module
–without-http_referer_module - 禁用 ngx_http_referer_module
–without-http_rewrite_module - 禁用 ngx_http_rewrite_module. 如果启用需要 PCRE 。
–without-http_proxy_module - 禁用 ngx_http_proxy_module
–without-http_fastcgi_module - 禁用 ngx_http_fastcgi_module
–without-http_memcached_module - 禁用 ngx_http_memcached_module
–without-http_browser_module - 禁用 ngx_http_browser_module
–http-proxy-temp-path=PATH - Set path to the http proxy temporary files
–http-fastcgi-temp-path=PATH - Set path to the http fastcgi temporary files
–without-http - 禁用 HTTP server（用作代理或反向代理）
–with-mail - 启用 IMAP4/POP3/SMTP 代理模块
–with-mail_ssl_module - 启用 ngx_mail_ssl_module
–with-openssl=DIR - Set path to OpenSSL library sources
```

3. 编译并安装

```shell
make && make install
```

4. 创建并设置 nginx运行账号（本章节是为了提高服务器的访问权限控制——可跳过，不影响个人使用）

   group add nginx

   user add -M -g nginx -s /sbin/nologin nginx（手动创建一个用户，不想让用户登录系统）

   cd /usr/local/src/nginx/config

   (注: M：不要自动建立用户的登入目录。)

   s是指定用户登入后所使用的she11。默认值为/bin/bash。如果不想让用户登录系统可以用-s /sbin/nologin此用户就不可以登录系统

5. 设置 nginx为系统服务


      在文件夹/lib/systemd/system新建文件 nginx.service
    
      文件创建：touch nginx.service
    
      vim /lib/systemd/system/nginx.service

   写入：

```shell
[Unit]
Description=nginx
After=network.target
[Service]
Type=forking
ExecStart=/usr/local/src/nginx/sbin/nginx
ExecReload=/usr/local/src/nginx/sbin/nginx -s reload
ExecStop=/usr/local/src/nginx/sbin/nginx -s stop
PrivateTmp=true
[Install]
WantedBy=multi-user.target
```

6. 启动nginx

   systemctl start nginx

   如果出现

   ![20220320124604.png](https://github.com/weiqh2000/blogImages/blob/main/20220320124604.png?raw=true)

   那么输入

   ```shell
   systemctl daemon-reload
   
   reboot
   ```

   再重新启动nginx即可

   ```shell
   systemctl restart nginx
   ```

   # nginx的config文件语法高亮

   1. 进入nginx的源码文件

   ```shell
   cd /usr/local/src/nginx-1.18.0/contrib/vim/syntax
   ```

   2. 将该文件夹下的nginx.vim复制到~/.vim/syntax/文件夹下，并且在~/.vim/filetype.vim文件中

   ```shell
   echo "au BufRead,BufNewFile /usr/local/src/nginx/conf/* set ft=nginx" >> /root/.vim/filetype.vim
   ```

   # 额外内容

   https://xinx.top/archives/45.html

   

   ```shell
   yum install kernel-headers
   ```

   Nginx官网 启动脚本

   https://www.nginx.com/resources/wiki/start/topics/examples/redhatnginxinit/
