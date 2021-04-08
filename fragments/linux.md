### collect some configure and manager info

#### 配置命令提示符
> 通过 export PS1="\u@\h \w>" 进行配置。 在/etc/profile 进行全局修改。  

#### df 命令不刷新存储空间。
> 查看正在deleted的进程ID  
> ls -ld /proc/*/fd/* 2>&1 | fgrep '(deleted)' 

#### linux 格式化划分分区
> fdisl -l 查看硬盘分区信息。  
> fdisk  /dev/vda  对硬盘进行分析  
> mkfs.ext3 /dev/vda1 对硬盘进行格式化  
> mount挂载。 修改fstab 启动挂载。  
#### lvm分区划分
> fdisk 分区后通过 t 命令修改id为8e
> pvcreate /dev/vda1  
> vgcreate  
> lvcreate  
