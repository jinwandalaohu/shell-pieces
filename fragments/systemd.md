#### 开机启动

1. 系统启动时 system 的 只执行/etc/systemd/system 目录里面的配置文件。
2. 用户自定义的服务放在 /usr/lib/systemd/system
3. systemctl enable httpd 实际是创建了一个符号链接

#### 配置文件

1. systemctl cat 查看配置文件
2. systemctl daemon-reload

```ini
[Unit]
Description=OpenSSH server daemon
Documentation=man:sshd(8) man:sshd_config(5)
After=network.target sshd-keygen.service
Wants=sshd-keygen.service

[Service]
EnvironmentFile=/etc/sysconfig/sshd
ExecStart=/usr/sbin/sshd -D $OPTIONS
ExecReload=/bin/kill -HUP $MAINPID
Type=simple
KillMode=process
Restart=on-failure
RestartSec=42s

[Install]
WantedBy=multi-user.target
```

##### Unit

1. Description 描述信息
2. After Before 描述启动顺序，不涉及依赖关系
   After=network.target ssh.service
3. Wants 弱依赖关系，不影响当前服务正常运行
4. Requires 强依赖关系， 如果依赖的服务退出，那么本服务也退出

##### Service

1. EnvironmentFile 环境参数文件，通过 key=value 配置，也可以用$key 在当前文件中获取，示例中的配置来至环境变量文件。
2. ExecReload 字段：重启服务时执行的命令  
   ExecStop 字段：停止服务时执行的命令  
   ExecStart  
   ExecStartPre 字段：启动服务之前执行的命令  
   ExecStartPost 字段：启动服务之后执行的命令  
   ExecStopPost 字段：停止服务之后执行的命令  
   Restart=always  
   RestartSec=5  
   Type:
   > Type=simple: 默认值  
   >  Type=forking : 以 fork 方式从父进程创建子进程，创建后父进程会退出。
   > Typr=oneshot : 一次性进程，systemd 会等当前服务退出，再继续向下执行。  
   >  Type=dbus 单前服务通过 D-Bus 启动。  
   >  Type=notify 单前服务启动完毕后会通知 systemd 在继续向下执行。  
   >  Type=idle 若有其他任务， 执行完毕后，当前服务才会运行。
   User=mysuer
   Group=mygroup
3. 如果配置出现空行，那么空行前的不生效。
4.
