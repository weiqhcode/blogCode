---
title: docker从安装到使用的常用命令
date: 2022-03-21 14:33:06
tags: 
- Docker
categories:
- 常用
---

清理Centos自带Docker版本
```shell
yum remove docker \
                  docker-client \
                  docker-client-latest \
                  docker-common \
                  docker-latest \
                  docker-latest-logrotate \
                  docker-logrotate \
                  docker-engine
```

安装Docker所需依赖
```shell
yum install -y yum-utils device-mapper-persistent-data lvm2
```

安装Docker-ce源
```shell
yum-config-manager --add-repo http://mirrors.aliyun.com/docker-ce/linux/centos/docker-ce.repo
```

更新缓存Yum缓存
```shell
yum makecache fast
```

下载Docker
```shell
yum -y install docker-ce
```

查看Docker版本并启动Docker服务
```shell
docker version
systemctl status docker
systemctl start docker
```

查看Docker镜像
```shell
docker images
docker images -qa
docker images --digests
```

设置Docker镜像源加速 (中科大镜像源)
```shell
mkdir -p /etc/docker
tee /etc/docker/daemon.json <<-'EOF'
{
  "registry-mirrors": ["https://docker.mirrors.ustc.edu.cn"]
}
EOF
```

查看Docker的GitHub列表目录

```shell
docker search [想查看的Docker应用]
例：
	[root@server ~]# docker search python
  NAME                                     DESCRIPTION                                     STARS     OFFICIAL   AUTOMATED
  python                                   Python is an interpreted, interactive, objec…   7701      [OK]       
  pypy                                     PyPy is a fast, compliant alternative implem…   330       [OK]       
  circleci/python                          Python is an interpreted, interactive, objec…   51                   
  hylang                                   Hy is a Lisp dialect that translates express…   45        [OK]       
  bitnami/python                           Bitnami Python Docker Image                     22                   [OK]
  cimg/python                                                                              5                    

```

