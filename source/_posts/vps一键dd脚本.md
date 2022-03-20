---
title: vps一键DD
date: 2022-03-19 13:23:26
tags: 
- vps
categories:
- vps
---



#### tips:

​	腾讯云自带的一些组件，会阻止我们DD安装操作系统（安装过程会报错，无法正确写入进磁盘），所以在DD安装操作系统前，必须要禁用、删除掉这些组件.



```
systemctl stop tat_agent
systemctl disable tat_agent
rm -rf /etc/systemd/system/tat_agent.service
rm -fr /usr/local/qcloud 
 
ps -A | grep agent
# 检查看是否还有腾讯云组件
# kill 这个进程
```

​	所有腾讯云组件都删除后，就可以DD安装Linux/Windows了.	

​	在使用 Centos 系统中使用 DD脚本，出现该错误

```
Press any key to continue...Error! Not Found grub.
```

​	重装切换到 Ubuntu 系统就不会出现该错误

​	腾讯云请先切换 Ubuntu 的镜像源至中科大镜像源，使用默认镜像源会导致下载缓慢



- ### 安装脚本所需的组件

```shell
#Debian/Ubuntu

 apt-get install -y xz-utils openssl gawk file curl wget 

#RedHat/CentOS

yum install -y xz openssl gawk file curl wget
```

- ### 使用CDN下载脚本

```shell
wget https://cdn.jsdelivr.net/gh/hiCasper/Shell@master/AutoReinstall.sh && chmod +x AutoReinstall.sh && bash AutoReinstall.sh
```

- #### 选择系统

![Snipaste_2022-03-19_15-41-16.jpg](https://github.com/weiqh2000/blogImages/blob/main/Snipaste_2022-03-19_15-41-16.jpg?raw=true)

​	运行到这步，控制台会断开连接，之后需要到腾讯云使用VNC连接配置



![Snipaste_2022-03-19_15-47-38.jpg](https://github.com/weiqh2000/blogImages/blob/main/Snipaste_2022-03-19_15-47-38.jpg?raw=true)



![Snipaste_2022-03-19_15-50-52.jpg](https://github.com/weiqh2000/blogImages/blob/main/Snipaste_2022-03-19_15-50-52.jpg?raw=true)



​	一路 Continue就好

![Snipaste_2022-03-19_15-53-03.jpg](https://github.com/weiqh2000/blogImages/blob/main/Snipaste_2022-03-19_15-53-03.jpg?raw=true)



Centos 

```
账号：root
默认密码：Pwd@CentOS
```

其他Liunx系统

```
账号：root
默认密码：Pwd@Linux
```









资料来源：

https://www.2331314.xyz/295.html

https://www.xugo.xyz/index.php/2021/12/11/%E8%85%BE%E8%AE%AF%E4%BA%91%E5%9B%BD%E5%86%85%E8%BD%BB%E9%87%8F-dd-%E7%B3%BB%E7%BB%9F/