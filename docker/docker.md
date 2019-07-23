
### 基本原理
docker本质上是宿主上的一个进程，docker通过namespace实现资源隔离，通过Cgroup实现资源限制，通过写时复制（copy-on-write）实现了高效的文件操作

| namespace | 系统调用          | 隔离内容             |
| --------- | ------------- | ---------------- |
| UTS       | Clone_NEWUTS  | 主机名与域名           |
| IPC       | Clone_NEWIPC  | 信号量、消息队列和共享内存    |
| PID       | Clone_NEWPID  | 进程编号             |
| NETWORK   | Clone_NEWNET  | 网络设备、网络栈、端口等     |
| MOUNT     | Clone_NEWNS   | 挂载点（文件系统）        |
| USER      | Clone_NEWUSER | 用户和用户组（3.8以后的内核） |



```shell
# docker 导出功能
docker save nginx > /tmp/nginx.tar.gz 

# docker 导入功能
docker load < /tmp/nginx.tar.gz 

```

```docker

vim  /usr/lib/systemd/system/docker.service 


docker deamon --help 

nameserver 
vi /etc/resolv.conf 


ctrl + P + Q --退出但不关闭当前容器 即docker ps可查看到 

docker run -it alpine sh
# -p 端口映射 第一个是宿主器的端口 第二是container的端口
docker run -it -d  -p 8088:80/udp --name myNginx nginx 
 
docker ps 

docker rm -f containerId/name
docker rmi imageId

--查看容器详情
docker inspect containerId/name

--进入容器
不推荐
docker attach containerId/name
推荐
docker exec -it containerId/name bin/bash
不推荐
使用namespace进入进程
# for nstat
yum install util-linux 
pid=`docker inspect --format "{{.State.Pid}}" $1`
nsenter -t $pid -m -u -i -n -p

# 日志
docker logs -f containerId/name 


# 本地提交镜像 
-- docker commit [comment] containerName  imageName:tagNumber
docker commit -m 'comment' ContainerName  hevo/imageName:v1 

# 上传镜像 
docker login 
docker images 获取制作的镜像ID
docker tag 镜像ID URL地址
docker push URL地址



# 容器之间互联 可以使得web2和web1之间能够IP互联  其中--link后面的别名很重要 
docker run --name web2 --link web1:alias -p 8080:80 containerName cmd


# docker默认网络
docker network ls 
bridge 
host  docker通讯完全走物理网卡 多个容器时会地址冲突
none

```




### docker跨主机互联
172.17.0.1是docker默认的网段，如果两个docker进程不加修改的话，都会同时是172.17.0.1形成回环，所以要修改docker的默认网段
``` shell
vi /usr/lib/systemd/system/docker.service 
--bip=172.17.42.1/24

systemctl deamon-reload
systemctl restart docker 


# 给两台宿主机添加路由
# node1 Server
route add -net docker1_IP:24 gw node2_IP

```

### 抓包
``` shell
yum install tcpdump -y 
tcpdump -i eno网卡 -vnn icmp
```



