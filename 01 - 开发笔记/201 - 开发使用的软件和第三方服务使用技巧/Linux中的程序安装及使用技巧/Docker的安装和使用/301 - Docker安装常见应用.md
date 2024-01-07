[Docker Hub地址](https://hub.docker.com/)

# 一、 Mysql

**1. Mysql镜像下载**
```bash
# 常用镜像版本拉取
docker pull mysql:latest
docker pull mysql:5.7
docker pull mysql:8

# arm64架构的芯片智能安装mysql8以上版本
docker pull arm64v8/mysql
docker pull mysql:8
```
**2. Mysql镜像安装**
```bash
# 最简单的Mysql安装,只需要指定密码和端口
sudo docker run -itd --name vm_mysql_20_3 -p 3306:3306 -e MYSQL_ROOT_PASSWORD=kepv1aUeg2 mysql:8
docker run -itd --name mysql_5.7 -p 3306:3306 -e MYSQL_ROOT_PASSWORD=kepv1aUeg2 mysql:5.7

```

<br/>

# 二、 Redis
**1.Redis镜像下载**
```bash
docker pull redis:latest
docker pull redis:6.0
docker pull redis:7.0

# Redis在版本 >= 6.0 以上支持用户名登录
docker pull arm64v8/redis
docker pull redis:6.0
```

**2.Redis镜像安装**
```bash
# 最简单的Redis安装,只需要指定端口
sudo docker run -itd --name vm_redis_20_3 -p 6379:6379 redis:6
```
<br/>

# 三、Nginx