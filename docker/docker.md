
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



