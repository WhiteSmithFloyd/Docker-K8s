
docker本质上是宿主上的一个进程，docker通过namespace实现资源隔离，通过Cgroup实现资源限制，通过写时复制（copy-on-write）实现了高效的文件操作


