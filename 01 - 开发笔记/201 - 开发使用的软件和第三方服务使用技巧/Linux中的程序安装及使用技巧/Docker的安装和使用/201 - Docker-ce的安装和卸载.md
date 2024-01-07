1. [Docker各个系统安装包下载地址](https://download.docker.com/linux/)
2. [Docker官方文档](https://docs.docker.com/get-docker/)
3. <font color="#31859b">如果下载时遇到网络问题可以参考使用代理工具：</font> [[各种Linux系统通过代理使用包管理工具]] 和 [[网络相关命令使用技巧]]


# 一、 CentOS
**安装Docker需要选择以下版本之一的CentOS镜像**
 - CentOS 7
 - CentOS 8 (stream)
 - CentOS 9 (stream)
  
```bash
# 一、安装软件依赖包
sudo yum install -y yum-utils device-mapper-persistent-data lvm2

# 二、添加软件仓库```
sudo yum-config-manager \
    --add-repo \
    https://download.docker.com/linux/centos/docker-ce.repo

# 三、使用yum安装
sudo yum install docker-ce

# 如果要安装指定的版本，可以直接使用yum list列出可用的版本
yum list docker-ce --showduplicates | sort -r
```
<br/>

## CentOS上卸载Docker
```bash
# 卸载 Docker Engine, CLI, containerd, Docker Compose安装包
sudo yum remove docker-ce docker-ce-cli containerd.io docker-buildx-plugin docker-compose-plugin docker-ce-rootless-extras

# 删除所有的镜像、容器和卷
sudo rm -rf /var/lib/docker
sudo rm -rf /var/lib/containerd
```
<br/>

# 二、Ubuntu
**安装Docker需要选择以下版本之一的Ubuntu镜像**
 - Ubuntu Kinetic 22.10
 - Ubuntu Jammy 22.04 (LTS)
 - Ubuntu Focal 20.04 (LTS)
 - Ubuntu Bionic 18.04 (LTS)

```bash
# 一、安装之前卸载旧版本的Docker
sudo apt-get remove docker docker-engine docker.io containerd runc

# 二、更新apt包索引并允许apt通过HTTPS使用存储库
sudo apt-get update

sudo apt-get install \
    ca-certificates \
    curl \
    gnupg

# 添加Docker的官方GPG密钥
sudo install -m 0755 -d /etc/apt/keyrings
curl -fsSL https://download.docker.com/linux/ubuntu/gpg | sudo gpg --dearmor -o /etc/apt/keyrings/docker.gpg
sudo chmod a+r /etc/apt/keyrings/docker.gpg

# 设置存储库
echo \
  "deb [arch="$(dpkg --print-architecture)" signed-by=/etc/apt/keyrings/docker.gpg] https://download.docker.com/linux/ubuntu \
  "$(. /etc/os-release && echo "$VERSION_CODENAME")" stable" | \
  sudo tee /etc/apt/sources.list.d/docker.list > /dev/null

# 更新包索引
sudo apt-get update

# 安装Docker-ce
sudo apt-get install docker-ce
```
<br/>

## Ubuntu上卸载Docker
```bash
# 卸载 Docker Engine, CLI, containerd, Docker Compose安装包
sudo apt-get purge docker-ce docker-ce-cli containerd.io docker-buildx-plugin docker-compose-plugin docker-ce-rootless-extras

# 删除所有的镜像、容器和卷
sudo rm -rf /var/lib/docker
sudo rm -rf /var/lib/containerd
```