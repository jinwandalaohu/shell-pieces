## <u><a href="#a">a</a></u> <u><a href="#b">b</a></u> <u><a href="#c">c</a></u> <u><a href="#d">d</a></u> <u><a href="#e">e</a></u> <u><a href="#f">f</a></u> <u><a href="#g">g</a></u>

## <u><a href="#h">h</a></u> <u><a href="#i">i</a></u> <u><a href="#j">j</a></u> <u><a href="#k">k</a></u> <u><a href="#l">l</a></u> <u><a href="#m">m</a></u> <u><a href="#n">n</a></u>

## <u><a href="#o">o</a></u> <u><a href="#p">p</a></u> <u><a href="#q">q</a></u> <u><a href="#r">r</a></u> <u><a href="#s">s</a></u> <u><a href="#t">t</a></u>

## <u><a href="#u">u</a></u> <u><a href="#v">v</a></u> <u><a href="#w">w</a></u> <u><a href="#x">x</a></u> <u><a href="#y">y</a></u> <u><a href="#z">z</a></u>

# [a](#a "a")

# [b](#b "b")

- basename
  > 移除路径的前缀,只保留只最后一个文件夹名称或文件名称
- basedir
  > 返回文件或文件夹所在的路径

```bash
basename /root/home # 返回home
basedir  /root/home # 返回/root
```

- [bash](fragments/bash.md)
  (https://www.yiibai.com/bash)
  (https://wangdoc.com/bash/intro.html)  
  _bash 相关信息_

# [c](#c "c")

- cpu
  > 查看 cpu 信息与虚拟化信息

```bash
cat /proc/cpuinfo | grep "physical id" | uniq | wc -l
cat /proc/cpuinfo | grep "cpu cores" | uniq
cat /proc/cpuinfo | grep 'model name' |uniq
# 查看cpu数量，查看cpu核数，查看cpu类型
dmidecode -s system-product-name
lscpu
# 查看是否为虚拟机

```

- cat

```bash
cat /proc/<pid>/maps |wc -l
监控，当前进程使用到的vm映射数量
```

- chage
  > 用来修改帐号和密码的有效期限。

```bash
chage -d $(date +%F) root
# 更新用户密码修改时间

chage -l root
# 查看root用户 密码策略。

```

- [conda](fragments/conda.md)

# [d](#d "d")

- [docker](fragments/docker.md)

# [e](#e "e")

# [f](#f "f")

- firewall-cmd
  > 参数 --time=5m 指定在 5 分钟后恢复修改。
  > 基本操作
  > 将流量分流到不同的区域，按照每个区域的规则去处理， 源区域优先于接口区域。 源区域指指定了源 IP， 接口区域指绑定的网卡接口（适配器）。
  >
  > 配置路径/etc/firewalld/ 与 /usr/lib/firewalld/ 后者为系统默认配置， 优先会使用前者的配置（远程配置可先修改配置文件再启动防火墙）。

```bash
# 查看所有区域
firewall-cmd --get-zones

#任何配置了一个网络接口和/或一个源的区域就是一个活动区域active zone。列出活动的区域：
firewall-cmd --get-active-zones
# 查看默认的区域
firewall-cmd --get-default-zone

# 查看开放的端口
firewall-cmd --zone=public --list-ports

# 查看添加的规则
firewall-cmd --zone=public --list-rich-rules

# 查看区域的规则，包含target
firewall-cmd --zone=public --list-all
```

> **查看端口状态**

```bash
# 开放单个端口，禁止某个端口。
firewall-cmd --zone=public --add-port=80/tcp --permanent
firewall-cmd --zone=public --remove-port=80/tcp --permanent
firewall-cmd --zone=public --add-port=8388-8389/tcp --permanent   # 开放端口范围

# 允许某个IP，
firewall-cmd --permanent --add-source=192.168.0.0/24

firewall-cmd --zone= public --query-port=80/tcp  # 查询某个端口
# 修改完成以后需要重新加载配置
firewall-cmd --reload


# 对 147.152.139.197 开放10000端口
firewall-cmd --permanent --zone=public --add-rich-rule='
        rule family="ipv4"
        source address="147.152.139.197/32"
        port protocol="tcp" port="10000" accept'

# 例子
firewall-cmd --permanent --zone=public --add-rich-rule='
        rule family="ipv4/ipv6"
        source address="ip/mask"    invet=true
        destination address-ip/mask invert=true
        server name = <service name>
        port protocol="tcp" port="10000" accept/drop/reject/log/adit'
# 拒绝端口：
firewall-cmd --permanent --zone=public --add-rich-rule='
              rule family="ipv4"
              source address="47.52.39.197/32"
              port protocol="tcp" port="10000" reject'

# 开放全部端口给IP
firewall-cmd --permanent --zone=public --add-rich-rule='
              rule family="ipv4"
              source address="192.168.0.1/32" accept';

# 开放全部端口给网段
firewall-cmd --permanent --zone=public --add-rich-rule='
              rule family="ipv4"
              source address="192.168.0.0/16" accept';

```

> **修改服务**
> 根据服务来判断，修改对应的配置文件是/etc/firewalld/zones/public.xml

```bash
# 查看全部支持的服务
$ firewall-cmd --get-service

# 查看开放的服务
$ firewall-cmd --list-service

# 绑定服务到区域,添加https
$ firewall-cmd --add-service=https --permanent

```

> **绑定接口到区域**，所有通过该接口的流量都按照该区域的规则。

```bash
firewall-cmd --zone=work --add-interface=ens33
# 绑定ens33 网卡到work区域，通过ens33的流量都按照work区域的规则。

firewall-cmd --zone=internal --change-interface=eth1
# 修改eth1 绑定到interna， （一个网卡只能同时绑定到一个接口）

firewall-cmd --get-zone-of-interface=ens33
# 查看网卡ens33 默认绑定到的区域。

firewall-cmd --zone=internal  --remove-interface=eth1

```

> **地址伪装** （添加伪装才能访问外网）

```bash
firewall-cmd --zone=work --add-masquerade
# 启用地址伪装（允许work区域的人上网）

firewall-cmd --zone=work --add-masquerade
# 禁止work区域的人上网

firewall-cmd --query-masquerade
```

> **禁止 ping**

```bash
firewall-cmd  --zone=work --add-icmp-block=echo-reply
# 禁止应答（不允许ping)

firewall-cmd --zone=worl --remove-icmp-block=echo-reply
# 允许ping

```

> **端口转发**

```bash
firewall-cmd [--zone=<zone>] --add-forward-port=port=<port>[-<port>]:proto=<protocol>{:toport=<port>[-port]|:toaddr=<adress>|:toport=<port>[-<port>]:toaddr=<adress>}
# 带to的是转成什么

firewall-cmd --zone=work --add-forward-port=port=80:proto=tcp:toport=8080:toaddr=192.168.1.1
# 访问work区域的80 端口的转发到192.168.1.1的8080端口。

firewall-cmd --zone=work --remove-forward-port=port=80:proto=tcp:toport=8080:toaddr=192.168.1.1
# 禁止这种转发。


```

> **启停服务**

```bash
    systemctl status firewalld        # 查看状态
    systemctl start firewalld         # 启动
    systemctl stop firewalld          #关闭
    systemctl enable firewalld        # 开机启动
    systemctl disable firewalld       # 取消开机启动
    firewall-cmd --state              # 查看状态
    firewall-cmd --reload             #重载配置，让服务生效
```

> 新加服务
> 在/usr/lib/firewalld/services，随便拷贝一个 xml 文件到一个新名字，比如 myservice.xml,把里面的 short 改为想要名字，重要的是修改 protocol 和 port。修改完保存，然后把新建的 service 添加到 firewalld。

```bash
firewall-cmd --permanent --add-service=myservice
```

# [g](#g "g")

- grep

```bash
grep -A 10
grep -B 10
grep -C 10
# -A 10 显示关键字之后的10行 -B 10 显示之前10行 -C 10 显示前后10行
```

- gpasswd
  > Linux 下工作组文件/etc/group 和/etc/gshadow 的管理工具，用于指定要管理的工作组。  
  >  -a : 添加用户到组
        -d : 从组删除用户
        -A：指定管理员
        -M：指定组成员和-A的用途差不多；
        -r：删除密码；
        -R：限制用户登入组，只有组中的成员才可以用newgrp加入该组
  > 添加用户到某一个组可以使用 usermod -G groupB userA 这个命令可以添加一个用户到指定的组，但是以前添加的组就会清空掉，所以想要添加一个用户到一个组，同时保留以前添加的组时，请使用 gpasswd 这个命令来添加操作用户

```bash
gpasswd -a userA groupB
# 将userA添加到groupB用户组里面：
```

- [git](fragments/git.md)

# [i](#i "i")

- iptbles
  > 在 RHEL7 里，默认是使用 firewalld 来管理 netfilter 子系统，不过底层调用的命令> 仍然是 iptables。firewalld 是 iptables 的前端控制器，用于实现持久的网络流量规则

```bash
yum install iptables-services
service iptables stop/start
# 安装iptables的服务,启停iptables服务

iptables -L
# getting all iptables rules lists

iptables -S
#  the status of the chains of iptables firewall

iptables -F
iptbales -F INPUT
# clean all iptables rules

iptables --policy INPUT DROP/ACCEPT
# to accept or revert default

iptables -A INPUT -p tcp --dport 80  -j ACCEPT
iptables -A INPUT -p tcp --dport 135 -j DROP
# 开启关闭端口

iptables -A OUTPUT -p icmp --icmp-type 8 -j DROP
iptables -I INPUT -p icmp --icmp-type 8 -j DROP
# 禁止ping，禁止被ping

iptables -A INPUT -p tcp -s 192.168.1.0/24 --dport 3306 -m state --state NEW,ESTABLISHED -j ACCEPT
# 只允许对应ip段访问mysql数据库

iptables -A INPUT -p tcp --dport 80 -m limit --limit 20/minute --limit-burst 100 -j ACCEPT
# 防止DDos攻击

iptables -N block-scan
iptables -A block-scan -p tcp —tcp-flags SYN,ACK,FIN,RST RST -m limit —limit 1/s -j RETURN
iptables -A block-scan -j DROP
# 防止被扫描 新建一个chain，然后配置规则

iptables -I INPUT -p tcp -m multiport --dport 26379:26381,16379:16381,12181 -m iprange --src-range 10.110.51.132-10.110.51.136 -j ACCEPT
# 配置iptables规则

iptables --version
# 查看iptables版本
```

# [j](#j "j")

# [k](#k "k")

- kill

```bash
kill -3 PID
# 打印java进程堆栈信息（同jstack）

ps -ef |grep hello |awk '{print $2}'|xargs kill -9

# kill ps 打印的进程
```

# [l](#l "l")

- last

  > 查看系统最后登录用户 IP

- lsof
  >

# [m](#m "m")

# [n](#n "n")

- netstat
  > 如果发现 fin_wait1 状态很多，并且 client ip 分布正常，那可能是有人用肉鸡进行 ddos 攻击、又或者最近的程序改动引起了问题。

```bash
netstat -ntlp
# 查看系统监听状态

netstat -n | awk '/^tcp/ {++S[$NF]} END {for(a in S) print a, S[a]}'
# 显示服务器各种状态及其数量

netstat -nat|grep ":80"|awk '{print $5}' |awk -F: '{print $1}' | sort| uniq -c|sort -n
# 将请求80服务的client ip按照连接数排序

netstat -atunlp
# 查看tcp、udp监听状态
```

- ntp
  > 时间同步配置，调整时间的快慢渐进的调整时间。

```bash
ntpstat
# 查看时间同步状态 synchronized说明已经同步

ntpq -p
# 查看同步信息，时间偏差等。

ntpdate -d ip
# 查看与IP的时间偏差，可用于查看时间服务是否开启。

```

- nmap

```bash
nmap -Pn 10.10.10.10
# 扫描主机所有端口状态。

```

- nc
  > 安装 yum install -y nmap-ncat

```bash
nc -l 10086 #在本机10086端口起监听
nc ip 10086 #向目标IP端口发送信息， 与上面一起使用用于测试网络是否连通。

```

# [o](#o "o")

# [p](#p "p")

- ps
  >

```bash
ps -eo pid,tty,user,comm,lstart,etime | grep redis
# 查看程序已执行的时间


ps -Lf <pid> |wc -l
# 查看某个进程中的线程数量

ps -ef |grep hello |awk '{print $2}'|xargs kill -9
# kill ps 打印的进程
```

- pmap
  > report memory map of a process  
  > pmap [ -x | -d ] [ -q ] pids

```bash


```

# [q](#q "q")

# [r](#r "r")

- rpm
  > RPM 软件包的管理工具。  
  > 用法: rpm [选项...]  
  > -a：查询所有套件；  
  > -b<完成阶段><套件档>+或-t <完成阶段><套件档>+：设置包装套件的完成阶> 段，并指定套件档的文件名称；  
  > -c：只列出组态配置文件，本参数需配合"-l"参数使用；  
  > -d：只列出文本文件，本参数需配合"-l"参数使用；  
  > -e<套件档>或--erase<套件档>：删除指定的套件；  
  > -f<文件>+：查询拥有指定文件的套件；  
  > -h 或--hash：套件安装时列出标记；  
  > -i：显示套件的相关信息；  
  > -i<套件档>或--install<套件档>：安装指定的套件档；  
  > -l：显示套件的文件列表；  
  > -p<套件档>+：查询指定的 RPM 套件档；  
  > -q：使用询问模式，当遇到任何问题时，rpm 指令会先询问用户；  
  > -R：显示套件的关联性信息；  
  > -s：显示文件状态，本参数需配合"-l"参数使用；  
  > -U<套件档>或--upgrade<套件档>：升级指定的套件档；  
  > -v：显示指令执行过程；  
  > -vv：详细显示指令执行过程，便于排错。

```bash
rpm -ivh *.rpm
# install rpm package

rpmrpm --force -ivh your-package.rpm
# 忽略报错直接安装

rmp -qa |grep xxx
rpm -ql tree
# 查询xxx软件包的安装信息

rpm -e tree          # 卸载

```

- rsync
  > 数据同步

```bash
  rsync -aXS /data/. /mnt/.   将数据从data目录同步到mnt目录
```

# [s](#s "s")

- set

```bash
setenforce 0
sed -i 's/^SELINUX=enforcing$/SELINUX=permissive/' /etc/selinux/config
# 关闭selinux

set -e #脚本 执行的时候 如果  出现了 返回值 为 非零 ，整个脚本 就会立即退出
set +e #如果 出现了 返回值 为 非零 将 会 继续 执行 下面的 脚本
# 在脚本中使用时 直接写在脚本内容第一行即可。
set -x #activate debugging from here
set +x #stop debugging from here

```

- shutdown
  > -c：取消关机操作;
  >
  > -f：重新启动时不执行 fsck;
  >
  > -F：重新启动时执行 fsck;
  >
  > -h：将系统关机;
  >
  > -k：只是送出信息给所有用户，但不会实际关机;
  >
  > -n：不调用 init 程序进行关机，而由 shutdown 自己进行;
  >
  > -r：shutdown 之后重新启动;
  >
  > -t<秒数>：送出警告信息和删除信息之间要延迟多少秒。

```bash
shutdown -h now     # 立即关机
shutdown -h 12:00   # 在12点关机
shutdown -c         # 取消关机

shutdown +5 “This System will be shutdown in 5 minutes” # 5分钟后关机
shutdown –r +3 “3分钟后关机重启”

```

- sysctl

```bash
cat /proc/sys/vm/max_map_count
sysctl -w vm.max_map_count=2048000
# 单个JVM能开启的最大线程数是/proc/sys/vm/max_map_count的设置数的一半.
```

# [t](#t "t")

- timedatectl
  > 查看系统的各种时间与时区信息  
  > timedatectl list-timezones 列出所有时区  
  > timedatectl set-local-rtc 1 将硬件时钟调整为与本地时钟一致, 0 为设置为 UTC 时间  
  > timedatectl set-timezone Asia/Shanghai 修改时区

# [u](#u "u")

- usermod

```bash
usermod -g root dsmart # 修改dsmart用户至 root组。

```

- ulimit

```bash
ulimit -a   # open file 一行为系统允许单个进程打开的最大句柄数
```

```bash
ulimit -n 2048 # 设置当前用户最大的句柄数，重启会失效。
```

> vim /etc/security/limits.conf

# [v](#v "v")

# [w](#w "w")

- which
- [wget](fragments/wget.md)
-

# [x](#x "x")

# [y](#y "y")

- [yum](../shellUtil/fragments/yum.md)
  > 包管理工具，可以同时又多个源，只要 repo 文件中[name]不同即可

```bash
yum repolist
# 列举出可用的源

reposync -r [base]
createrepo -v  # 在下载的yum源文件中的Packages文件夹中执行，报错移除软件包
# 配置本地yum源文件
yum clean all
yum makecache fast
# 下载源的所有包到当前文件夹，然后可设置为本地的yum源

yum update #升级系统内核与所有包
yum upgrade #升级所有包
```

# [z](#z "z")
