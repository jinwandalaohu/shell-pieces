echo 511 > /proc/sys/net/core/somaxconn
vi /etc/sysctl.conf
在这里面添
net.core.somaxconn= 1024
vm.overcommit_memory = 1

然后执行
sysctl -p

docker :
宿主机： vm.overcommit_memory = 1
启动命令： --sysctls: - net.core.somaxconn=1024

## 配置
1.设置redis 客户端空闲 N 秒后关闭连接（0 表示禁用）timeout 0

2.redis的配置文件redis.conf中设置tcp-keepalive时间为60s  (tcp 连接存活时间)

3.程序配置文件中修改 spring.redis.lettuce.shutdown-timeout: 100(redis超时时间)

作者：慢慢走_2a3a
链接：https://www.jianshu.com/p/c608a7e9c86e
来源：简书
著作权归作者所有。商业转载请联系作者获得授权，非商业转载请注明出处。
