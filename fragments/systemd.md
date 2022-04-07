#### 开机启动
1. 系统启动时 system的 只执行/etc/systemd/system 目录里面的配置文件。  
2. 用户自定义的服务放在 /usr/lib/systemd/system  
3. systemctl enable httpd 实际是创建了一个符号链接  
#### 配置文件
1. systemctl cat 查看配置文件  
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
3. Wants 弱依赖关系，不影响当前服务正常运行  
4. Requires 强依赖关系， 如果依赖的服务退出，那么本服务也退出  
##### Service
1. EnvironmentFile 环境参数文件，通过key=value配置，也可以用$key在当前文件中获取，示例中的配置来至环境变量文件。  
2.  ExecReload字段：重启服务时执行的命令   
    ExecStop字段：停止服务时执行的命令  
    ExecStartPre字段：启动服务之前执行的命令   
    ExecStartPost字段：启动服务之后执行的命令  
    ExecStopPost字段：停止服务之后执行的命令  
3. 如果配置出现空行，那么空行前的不生效。  
4. 