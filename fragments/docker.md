# docker

* docker run
> 参数  --restart=always  当docker 服务重启时重启容器。

```bash
# 停、起所有docker容器
docker stop  $(docker ps -a | awk '{ print $1}' | tail -n +2)
docker start $(docker ps -a | awk '{ print $1}' | tail -n +2)
```
## docker默认根路径
```
 docker info |grep 'Docker Root Dir' 查看docker的root dir。
```
## 修改docker根路径
1. 停止所有docker容器
```
docker stop $(docker ps -a | awk '{ print $1}' | tail -n +2)
```
2. 停止docker服务
```
service docker stop
```
3. 迁移docker数据
```bash
mkdir /data/service/docker -p
mv /var/lib/docker /data/service/docker/

```
4. 修改docker默认的存储位置
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
5. 启动docker服务，启动所有容器
```bash
service docker start
docker start $(docker ps -a | awk '{ print $1}' | tail -n +2)

```
6. docker服务容器自动启动与停止
```bash
systemctl enable docker.service ## docker 服务开机启动自动启动
systemctl disable docker.service ## 关闭开机启动
docker run --restart=always ## 启动时 容器自动启动
docker update --restart=always <CONTAINER ID> ## 启动后  修改容器自动启动
docker update --restart=on-failure:3 [容器名] ## 自动启动3次，失败后放弃
```
> --restart参数有3个可选值 :  no,on-failure,always

## 备份docker
> docker容器路径 /var/lib/docker/containers
> 备份容器配置
```bash
cd /var/lib/docker/containers/de9c6501cdd3
cp hostconfig.json hostconfig.json.bak
cp config.v2.json config.v2.json.bak
```
## 创建docker镜像[docker](docker/dockerfile.md)

## 知识点
> docker 通过namespace进行隔离，通过Cgroup进行限制，Mount namespace与rootfs构建文件系统。
> 多个层通过 AUFS 进行联合挂载。
* AUFS
  ![layer](../images/docker_layer.png)
```

```