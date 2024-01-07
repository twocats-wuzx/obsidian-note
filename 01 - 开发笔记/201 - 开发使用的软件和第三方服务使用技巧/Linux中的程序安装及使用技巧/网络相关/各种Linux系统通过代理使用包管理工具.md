# 一、CentOS
```bash
# yum 使用代理服务器进行更新
	# 需要将代理配置直接配置到/etc/yum.conf文件中

vim /etc/yum.config
proxy=http://proxy_host:proxy_port

# 例如
echo "proxy=http://192.168.20.2:7890" >> /etc/yum.conf

# 然后就可以正常的更新、升级和下载数据了
sudo yum install openvpn3
sudo yum update
sudo yum upgrade
```
<br/>

# 二、Ubuntu
```bash
# apt使用代理服务器进行更新
sudo apt-get -o Acquire::http::proxy="http://192.168.20.2:7890" update

# apt包管理工具使用代理服务器进行下载程序
sudo apt-get -o Acquire::http::proxy="http://192.168.20.2:7890" install docker-ce
```
