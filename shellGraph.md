
  ## <u><a href="#a">a</a></u>    <u><a href="#b">b</a></u>   <u><a href="#c">c</a></u>    <u><a href="#d">d</a></u>    <u><a href="#e">e</a></u>    <u><a href="#f">f</a></u>    <u><a href="#g">g</a></u>  
  ## <u><a href="#h">h</a></u>    <u><a href="#i">i</a></u>    <u><a href="#j">j</a></u>    <u><a href="#k">k</a></u>    <u><a href="#l">l</a></u>    <u><a href="#m">m</a></u>    <u><a href="#n">n</a></u>  
  ## <u><a href="#o">o</a></u>    <u><a href="#p">p</a></u>    <u><a href="#q">q</a></u>    <u><a href="#r">r</a></u>    <u><a href="#s">s</a></u>    <u><a href="#t">t</a></u>  
  ## <u><a href="#u">u</a></u>    <u><a href="#v">v</a></u>    <u><a href="#w">w</a></u>    <u><a href="#x">x</a></u>    <u><a href="#y">y</a></u>    <u><a href="#z">z</a></u>  
 _____   
# [a](#a)
# [b](#b "b")
# [c](#c "c")
* cpu
> 查看cpu信息与虚拟化信息
```bash
cat /proc/cpuinfo | grep "physical id" | uniq | wc -l
cat /proc/cpuinfo | grep "cpu cores" | uniq
cat /proc/cpuinfo | grep 'model name' |uniq
# 查看cpu数量，查看cpu核数，查看cpu类型
dmidecode -s system-product-name
lscpu
# 查看是否为虚拟机

```
* cat
```bash
cat /proc/<pid>/maps |wc -l
监控，当前进程使用到的vm映射数量
```
# [d](#d "d")
# [e](#e "e")
# [f](#f "f")
# [g](#g "g")
* grep
  
```bash
grep -A 10
grep -B 10
grep -C 10
# -A 10 显示关键字之后的10行 -B 10 显示之前10行 -C 10 显示前后10行
```
* gpasswd
> Linux下工作组文件/etc/group和/etc/gshadow的管理工具，用于指定要管理的工作组。  
>     -a : 添加用户到组  
      -d : 从组删除用户  
      -A：指定管理员  
      -M：指定组成员和-A的用途差不多；  
      -r：删除密码；  
      -R：限制用户登入组，只有组中的成员才可以用newgrp加入该组  
> 添加用户到某一个组可以使用  usermod -G groupB userA 这个命令可以添加一个用户到指定的组，但是以前添加的组就会清空掉，所以想要添加一个用户到一个组，同时保留以前添加的组时，请使用gpasswd这个命令来添加操作用户
```bash 
gpasswd -a userA groupB 
# 将userA添加到groupB用户组里面： 
```
* [git](fragments/git.md)
# [i](#i "i")
* iptbles
> 在RHEL7里，默认是使用firewalld来管理netfilter子系统，不过底层调用的命令> 仍然是iptables。firewalld是iptables的前端控制器，用于实现持久的网络流量规则
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
* kill
```bash
kill -3 PID
# 打印java进程堆栈信息（同jstack）

ps -ef |grep hello |awk '{print $2}'|xargs kill -9  

# kill ps 打印的进程
```

# [l](#l "l")
* last
> 查看系统最后登录用户IP
# [m](#m "m")
# [n](#n "n")
* netstat
> 如果发现fin_wait1状态很多，并且client ip分布正常，那可能是有人用肉鸡进行ddos攻击、又或者最近的程序改动引起了问题。
````bash
netstat -ntlp 
# 查看系统监听状态

netstat -n | awk '/^tcp/ {++S[$NF]} END {for(a in S) print a, S[a]}' 
# 显示服务器各种状态及其数量

netstat -nat|grep ":80"|awk '{print $5}' |awk -F: '{print $1}' | sort| uniq -c|sort -n
# 将请求80服务的client ip按照连接数排序

netstat -atunlp
# 查看tcp、udp监听状态
````
* ntp
> 时间同步配置，调整时间的快慢渐进的调整时间。
```bash
ntpstat
# 查看时间同步状态 synchronized说明已经同步 

ntpq -p
# 查看同步信息，时间偏差等。

```
# [o](#o "o")
# [p](#p "p")
* ps
> 
```bash
ps -eo pid,tty,user,comm,lstart,etime | grep redis
# 查看程序已执行的时间


ps -Lf <pid> |wc -l
# 查看某个进程中的线程数量

ps -ef |grep hello |awk '{print $2}'|xargs kill -9 [href](#href)
# kill ps 打印的进程
```
* pmap
> report memory map of a process  
> pmap [ -x | -d ] [ -q ] pids
```bash


```
# [q](#q "q")
# [r](#r "r")
* rpm
> RPM软件包的管理工具。  
> 用法: rpm [选项...]    
> -a：查询所有套件；  
> -b<完成阶段><套件档>+或-t <完成阶段><套件档>+：设置包装套件的完成阶> 段，并指定套件档的文件名称；  
> -c：只列出组态配置文件，本参数需配合"-l"参数使用；  
> -d：只列出文本文件，本参数需配合"-l"参数使用；  
> -e<套件档>或--erase<套件档>：删除指定的套件；  
> -f<文件>+：查询拥有指定文件的套件；  
> -h或--hash：套件安装时列出标记；  
> -i：显示套件的相关信息；  
> -i<套件档>或--install<套件档>：安装指定的套件档；  
> -l：显示套件的文件列表；  
> -p<套件档>+：查询指定的RPM套件档；  
> -q：使用询问模式，当遇到任何问题时，rpm指令会先询问用户；  
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
# [s](#s "s")
* set 
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
* sysctl
```bash
cat /proc/sys/vm/max_map_count
sysctl -w vm.max_map_count=2048000
# 单个JVM能开启的最大线程数是/proc/sys/vm/max_map_count的设置数的一半.
```
# [t](#t "t")
# [u](#u "u")
# [v](#v "v")
# [w](#w "w")
*  which
*  [wget](fragments/wget.md)
* 
# [x](#x "x")
# [y](#y "y")
* yum 
> 包管理工具，可以同时又多个源，只要repo文件中[name]不同即可
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