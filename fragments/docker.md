1. [docker](#docker)
   1. [docker 默认根路径](#docker-默认根路径)
   2. [修改 docker 根路径](#修改-docker-根路径)
   3. [备份 docker](#备份-docker)
   4. [创建 docker 镜像 docker](#创建-docker-镜像docker)
   5. [在 docker 内执行命令](#在-docker-内执行命令)
   6. [知识点](#知识点)

# docker

docker 默认情况下, docker 启动后参数中如果加了端口映射, 就会自动将端口开放给所有网络设备访问,  
并且这种情况下即使在本机的系统防火墙中加规则也无效, 因为 docker 会自动添加一个优先级最高的针对这个映射端口全开放规则,
这样就需要在 docker 启动时添加参数来禁止 docker 对本机防火墙的操作.
修改 /usr/lib/systemd/system/docker.service ExecStart=/usr/bin/dockerd --iptables=false xxxxxxx

服务器 IP 更改之后，linux 数据包转发配置 net.ipv4.ip_forward 会变为 0，即关闭状态，需要修改为 1
sysctl -w net.ipv4.ip_forward=1 或 vi /etc/sysctl.conf sysctl -p 生效

- docker run
  > 参数 --restart=always 当 docker 服务重启时重启容器。

```bash
# 停、起所有docker容器
docker stop  $(docker ps -a | awk '{ print $1}' | tail -n +2)
docker start $(docker ps -a | awk '{ print $1}' | tail -n +2)
```

## docker 默认根路径

```
 docker info |grep 'Docker Root Dir' 查看docker的root dir。
```

## 修改 docker 根路径

1. 停止所有 docker 容器

```
docker stop $(docker ps -a | awk '{ print $1}' | tail -n +2)
```

2. 停止 docker 服务

```
service docker stop
```

3. 迁移 docker 数据

```bash
mkdir /data/service/docker -p
mv /var/lib/docker /data/service/docker/

```

4. 修改 docker 默认的存储位置

```bash
vim /etc/docker/daemon.json

{
    "graph": "/home/server/docker"
}

systemctl daemon-reload # 重新加载配置

#或者修改docker.service配置文件，使用 --graph 参数指定存储位置
vim /usr/lib/systemd/system/docker.service
ExecStart=/usr/bin/dockerd -H fd:// --containerd=/run/containerd/containerd.sock --graph /data/services/docker
```

5. 启动 docker 服务，启动所有容器

```bash
service docker start
docker start $(docker ps -a | awk '{ print $1}' | tail -n +2)

```

6. docker 服务容器自动启动与停止

```bash
systemctl enable docker.service ## docker 服务开机启动自动启动
systemctl disable docker.service ## 关闭开机启动
docker run --restart=always ## 启动时 容器自动启动
docker update --restart=always <CONTAINER ID> ## 启动后  修改容器自动启动
docker update --restart=on-failure:3 [容器名] ## 自动启动3次，失败后放弃
```

> --restart 参数有 3 个可选值 : no,on-failure,always

## 备份 docker

> docker 容器路径 /var/lib/docker/containers
> 备份容器配置

```bash
cd /var/lib/docker/containers/de9c6501cdd3
cp hostconfig.json hostconfig.json.bak
cp config.v2.json config.v2.json.bak
```

## 创建 docker 镜像[docker](docker/dockerfile.md)

## 在 docker 内执行命令

1. 进入容器内通过命令行执行多个命令：`docker exec -it container_name /bin/sh`
2. 通过镜像执行，执行后删除容器`docker run -i --rm image-name cat /path/to/config`
3. 在容器外部执行`docker exec container_name ls /` 注:

## 知识点

> docker 通过 namespace 进行隔离，通过 Cgroup 进行限制，Mount namespace 与 rootfs 构建文件系统。
> 多个层通过 AUFS 进行联合挂载。

- AUFS
  ![layer](../images/docker_layer.png)

```

```
