# docker

* docker run
> 参数  --restart=always  当docker重启时重启容器。
> 

```

```


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
1. 迁移docker数据
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


## 备份docker
> docker容器路径 /var/lib/docker/containers
> 备份容器配置
```bash
cd /var/lib/docker/containers/de9c6501cdd3
cp hostconfig.json hostconfig.json.bak
cp config.v2.json config.v2.json.bak
```
## [创建docker镜像](https://blog.csdn.net/qq_29999343/article/details/78318397)
1. 编写dockerfile
> dockerfiel指令  

 
|指令 | 功能 |
|:----|:---------------------|
|<a href="#FROM">FROM</a>| 设置镜像使用的基础镜像|
|MAINTAINER|设置镜像作者|
|RUN|镜像编译时运行的脚本|
|CMD|设置容器的启动命令|
|LABEL|设置镜像的标签|
|EXPOESE|设置镜像暴露的端口|
|ENV|设置容器的环境变量|
|ADD|编译镜像时复制文件到镜像中|
|COPY|编译镜像时复制文件到镜像中|
|ENTRYPOINT|设置容器的入口程序|
|VOLUME|设置容器的挂载卷|
|USER|设置运行RUN CMD ENTRYPOINT的用户名|
|WORKDIR| 设置 RUN CMD ENTRIPOINT COPY ADD 指令的工作目录|
|ARG|设置编译镜像时加入的参数|
|ONBUILD|设置镜像的ONBUILD指令|
|STOPSIGNAL| 设置容器的退出信号量|
|HEALTHCHECK|容器健康状况检查命令|


> 指令：[FROM](#FROM)
功能描述：设置基础镜像  
语法：FROM < image>[:< tag> | @< digest>]  
提示：镜像都是从一个基础镜像（操作系统或其他镜像）生成，可以在一个Dockerfile中添加多条FROM指令，一次生成多个镜像
注意：如果忽略tag选项，会使用latest镜像  

> 指令：MAINTAINER    
功能描述：设置镜像作者
语法：MAINTAINER < name>


> 指令：RUN    
功能描述：  
语法：RUN < command>  
           RUN ["executable","param1","param2"]  
提示：RUN指令会生成容器，在容器中执行脚本，容器使用当前镜像，脚本指令完成后，Docker Daemon会将该容器提交为一个中间镜像，供后面的指令使用  
补充：RUN指令第一种方式为shell方式，使用/bin/sh -c < command>运行脚本，可以在其中使用\将脚本分为多行  
           RUN指令第二种方式为exec方式，镜像中没有/bin/sh或者要使用其他shell时使用该方式，其不会调用shell命令  
例子：RUN source \$HOME/.bashrc;  
          echo \$HOME  
          RUN ["/bin/bash","-c","echo hello"]  
          &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;RUN ["sh","-c","echo","$HOME"]  
      使用第二种方式调用shell读取环境变量

>     指令：CMD  
功能描述：设置容器的启动命令  
语法：CMD [“executable","param1","param2"]  
           CMD [“param1","param2"]  
           CMD < command>  
提示：CMD第一种、第三种方式和RUN类似，第二种方式为ENTRYPOINT参数方式，为entrypoint提供参数列表  
注意：Dockerfile中只能有一条CMD命令，如果写了多条则最后一条生效

> 指令：LABEL  
功能描述：设置镜像的标签  
延伸：镜像标签可以通过docker inspect查看  
格式：LABEL < key>=< value> < key>=< value> …  
提示：不同标签之间通过空格隔开  
注意：每条指令都会生成一个镜像层，Docker中镜像最多只能有127层，如果超出Docker Daemon就会报错，如LABEL ..=.. <假装这里有个换行> LABEL ..=..合在一起用空格分隔就可以减少镜像层数量，同样，可以使用连接符\将脚本分为多行  
          镜像会继承基础镜像中的标签，如果存在同名标签则会覆盖

> 指令：EXPOSE  
功能描述：设置镜像暴露端口，记录容器启动时监听哪些端口  
语法：EXPOSE < port> < port> …  
延伸：镜像暴露端口可以通过docker inspect查看  
提示：容器启动时，Docker Daemon会扫描镜像中暴露的端口，如果加入-P参数，Docker Daemon会把镜像中所有暴露端口导出，并为每个暴露端口分配一个随机的主机端口（暴露端口是容器监听端口，主机端口为外部访问容器的端口）
注意：EXPOSE只设置暴露端口并不导出端口，只有启动容器时使用-P/-p才导出端口，这个时候才能通过外部访问容器提供的服务

> 指令：ENV  
功能描述：设置镜像中的环境变量  
语法：ENV < key>=< value>…|< key> < value>  
注意：环境变量在整个编译周期都有效，第一种方式可设置多个环境变量，第二种方式只设置一个环境变量  
提示：通过\${变量名}或者 \$变量名使用变量，使用方式\${变量名}时可以用\${变量名:-default} \${变量名:+cover}设定默认值或者覆盖值  
          ENV设置的变量值在整个编译过程中总是保持不变的

> 指令：ADD  
功能描述：复制文件到镜像中  
语法：ADD < src>… < dest>|[“< src>",… “< dest>"]  
注意：当路径中有空格时，需要使用第二种方式  
          当src为文件或目录时，Docker Daemon会从编译目录寻找这些文件或目录，而dest为镜像中的绝对路径或者相对于WORKDIR的路径
提示：src为目录时，复制目录中所有内容，包括文件系统的元数据，但不包括目录本身  
          src为压缩文件，并且压缩方式为gzip,bzip2或xz时，指令会将其解压为目录  
          如果src为文件，则复制文件和元数据  
          如果dest不存在，指令会自动创建dest和缺失的上级目录  

> 指令：COPY  
功能描述：复制文件到镜像中  
语法：COPY < src>… < dest>|[“< src>",… “< dest>"]  
提示：指令逻辑和ADD十分相似，同样Docker Daemon会从编译目录寻找文件或目录，dest为镜像中的绝对路径或者相对于WORKDIR的路径

> 指令：ENTRYPOINT  
功能描述：设置容器的入口程序  
语法：ENTRYPOINT [“executable","param1","param2"]  
           ENTRYPOINT command param1 param2（shell方式）  
提示：入口程序是容器启动时执行的程序，docker run中最后的命令将作为参数传递给入口程序  
           入口程序有两种格式：exec、shell，其中shell使用/bin/sh -c运行入口程序，此时入口程序不能接收信号量  
           当Dockerfile有多条ENTRYPOINT时只有最后的ENTRYPOINT指令生效  
           如果使用脚本作为入口程序，需要保证脚本的最后一个程序能够接收信号量，可以在脚本最后使用exec或gosu启动传入脚本的命令  
          注意：通过shell方式启动入口程序时，会忽略CMD指令和docker run中的参数  
           为了保证容器能够接受docker stop发送的信号量，需要通过exec启动程序；如果没有加入exec命令，则在启动容器时容器会出现两个进程，并且使用docker stop命令容器无法正常退出（无法接受SIGTERM信号），超时后docker stop发送SIGKILL，强制停止容器  
           例子：FROM ubuntu <换行> ENTRYPOINT exec top -b

> 指令：VOLUME  
功能描述：设置容器的挂载点  
语法：VOLUME ["/data"]  
           VOLUME /data1 /data2  
提示：启动容器时，Docker Daemon会新建挂载点，并用镜像中的数据初始化挂载点，可以将主机目录或数据卷容器挂载到这些挂载点

> 指令：USER  
功能描述：设置RUN CMD ENTRYPOINT的用户名或UID  
语法：USER < name>  

> 指令：WORKDIR  
功能描述：设置RUN CMD ENTRYPOINT ADD COPY指令的工作目录  
语法：WORKDIR < Path>  
提示：如果工作目录不存在，则Docker Daemon会自动创建  
          Dockerfile中多个地方都可以调用WORKDIR，如果后面跟的是相对位置，则会跟在上条WORKDIR指定路径后（如WORKDIR /A   WORKDIR B   WORKDIR C，最终路径为/A/B/C）

> 指令：ARG   
功能描述：设置编译变量  
语法：ARG < name>[=< defaultValue>]  
注意：ARG从定义它的地方开始生效而不是调用的地方，在ARG之前调用编译变量总为空，在编译镜像时，可以通过docker build –build-arg < var>=< value>设置变量，如果var没有通过ARG定义则Daemon会报错  
          可以使用ENV或ARG设置RUN使用的变量，如果同名则ENV定义的值会覆盖ARG定义的值，与ENV不同，ARG的变量值在编译过程中是可变的，会对比使用编译缓存造成影响（ARG值不同则编译过程也不同）  
例子：ARG CONT_IMAG_VER    
           RUN echo $CONT_IMG_VER  
           ARG CONT_IMAG_VER   
            RUN echo hello   
          当编译时给ARG变量赋值hello，则两个Dockerfile可以使用相同的中间镜像，如果不为hello，则不能使用同一个中间镜像

> 指令：ONBUILD  
功能描述：设置自径想的编译钩子指令    
语法：ONBUILD [INSTRUCTION]  
提示：从该镜像生成子镜像，在子镜像的编译过程中，首先会执行父镜像中的ONBUILD指令，所有编译指令都可以成为钩子指令

> 指令：STOPSIGNAL  
功能描述：设置容器退出时，Docker Daemon向系统发送的信号量  
语法：STOPSIGNAL signal  
提示：信号量可以是数字或者信号量的名字，如9或者SIGKILL，信号量的数字说明在Linux系统管理中有简单介绍

> HEALTHCHECK  
> 功能描述:容器健康状况检查命令  
> 语法有两种：  
> 1. HEALTHCHECK [OPTIONS] CMD command  
> 2. HEALTHCHECK NONE  
> 第一个的功能是在容器内部运行一个命令来检查容器的健康状况
> 第二个的功能是在基础镜像中取消健康检查命令  
> [OPTIONS]的选项支持以下三中选项：  
    --interval=DURATION 两次检查默认的时间间隔为30秒    
    --timeout=DURATION 健康检查命令运行超时时长，默认30秒  
    --retries=N 当连续失败指定次数后，则容器被认为是不健康的，状态为unhealthy，默认次数是3  
> 注意：  
HEALTHCHECK命令只能出现一次，如果出现了多次，只有最后一个生效。  
CMD后边的命令的返回值决定了本次健康检查是否成功，具体的返回值如下：  
0: success - 表示容器是健康的  
1: unhealthy - 表示容器已经不能工作了  
2: reserved - 保留值  
例子：  
HEALTHCHECK --interval=5m --timeout=3s \  
CMD curl -f http://localhost/ || exit 1  
健康检查命令是：curl -f http://localhost/ || exit 1  
两次检查的间隔时间是5秒  
命令超时时间为3秒  

> 补充：ONBUILD流程   
  编译时，读取所有ONBUILD镜像并记录下来，在当前编译过程中不执行指令  
生成镜像时将所有ONBUILD指令记录在镜像的配置文件OnBuild关键字中  
子镜像在执行FROM指令时会读取基础镜像中的ONBUILD指令并顺序执行，如果执行过程中失败则编译中断；当所有ONBUILD执行成功后开始执行子镜像中的指令  
子镜像不会继承基础镜像中的ONBUILD指令


> 补充：CMD ENTRYPOINT和RUN的区别  
       RUN指令是设置编译镜像时执行的脚本和程序，镜像编译完成后，RUN指令的生命周期结束  
       容器启动时，可以通过CMD和ENTRYPOINT设置启动项，其中CMD叫做容器默认启动命令，如果在docker run命令末尾添加command，则会替换镜像中CMD设置的启动程序；ENRTYPOINT叫做入口程序，不能被docker run命令末尾的command替换，而是将command当作字符串，传递给ENTRYPOINT作为参数

```

```