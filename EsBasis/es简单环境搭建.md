### 01.ES docker环境搭建

1. 设置国内镜像地址，为docker加速 <br>
```sudo vim /etc/docker/daemon.json
{
    "registry-mirrors": ["https://wixr7yss.mirror.aliyuncs.com"] 
}
sudo systemctl daemon-reload
sudo systemctl restart docker
```
2. 创建网段，为了让后期同一机器上的es容器处于同一个网段上，这样我们使用一台机器就可以创建一个es集群<br>
```docker network create --subnet=172.18.0.0/16 esNet```
3. 拉取es镜像（我们拉取一个包含kibana的es镜像，先进行es的入门）<br>
```docker pull nshou/elasticsearch-kibana```
4. 创建es容器<br>
```docker run -e ES_JAVA_OPTS="-Xms256m -Xmx256m" --restart always  --privileged=true -d -p 9200:9200 -p 9300:9300 -p 5601:5601 --net esNet --ip 172.18.0.8  --name es01 6a72f19c1678```
5. 参数解释<br>
ES_JAVA_OPTS="-Xms256m -Xmx256m"  //设置JVM内存，默认2G <br>
--restart always  //自动重启<br>
--privileged=true //设置权限<br>
-d //后台启动<br>
-p //端口号<br>
--net //设置网段<br>
--name //为容器设置名字<br>
6a72f19c1678 //镜像的 IMAGE ID<br>
6. 开放端口号<br>
```
# 开放端口
firewall-cmd --zone=public --add-port=9200-9202/tcp --permanent
firewall-cmd --zone=public --add-port=9300-9302/tcp --permanent
firewall-cmd --zone=public --add-port=5601/tcp --permanent
# 重启防火墙
systemctl status firewalld
# 查看已经开放的端口号
firewall-cmd --zone=public --list-ports
```
7. 查看es容器和kinaba是否创建成功，换成自己的主机地址，如果能够正常显示，则表明创建成功！<br>
7.1 浏览器输入：192.168.27.**:5601<br>
7.2 浏览器输入：192.168.27.**:9200<br>
![kinaba成功开启展示](https://github.com/Maxwellwk/ElasticsearchNotes/blob/main/picture/kinaba01.png)


