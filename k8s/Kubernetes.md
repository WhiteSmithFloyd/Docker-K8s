# Kubernetes

`https://www.cnblogs.com/liyongsan/p/10560278.html`

+ Master/node 模型
  - master: API Server, Scheduler, Controller-Manager
  - node: Kubelet, docker
+ Pod
  - Label: key=value
  - Label Selector 
  
+ Pad
  - 自主式Pod （不推荐）
  - 控制器管理的Pod
    - Replication Controller (OLD)
    - ReplicSet
    - Deployment
    - StatefulSet
    - DaemonSet
    - Job, Ctonjob  HPA(HorizontalPodAutoscaler)



安装
使用master中三个组件作为node的模式安装集群
master节点：
1. 首先配置docker repo
配置yum 的repo for docker 和 kubernate，配置为阿里云
kubernetes.repo文件
```
kubernetes
name=Kubenetes Repo
baseurl=https://mirrors.aliyun.com/
gpgcheck=0
gpgkey=
enabled=1
```

使用 `yum repolist `来检查

安装 docker kubelet 等
yum install docker-ce kubelet  kubeadm  kubectl 

1. 启动docker服务
`/usr/lib/systemd/system/docker.service`
```
Environment="HTTPS_PROXY=http://www.ik8s.io:10080"
Environment="NOPROXY=127.0.0.0/8,172.20.0.0/16"
```
```
systemctl daemon-reload 
systemctl star docker
docker info
```
check iptables
```
cat /pro/sys/net/bridge/bridge-nf-call-ip6tables
1
cat /pro/sys/net/bridge/bridge-nf-call-iptables
1
```

2. 启动kubernetes服务
```
kubeadm init --kubernetes-version=v1.11.1 --pod-network-cidr=10.244.0.0/16 
--service-cidr=10.96.0.0/12 
```

关闭swap
```
vi /etc/syconfig/kubelet
KUBELET_EXTRA_ARGS="--fail-swap-on=false"

kubeadm init 后面加上 --ignore-preflight-errors=Swap  
```


