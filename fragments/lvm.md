### fdisk 进行分区 （分区完成后使用 partprobe 使分区生效）
#### lvm分区划分
> fdisk 分区后通过 t 命令修改id为8e
> pvcreate /dev/vda1  
> vgcreate  
> lvcreate  

### 

### 扩容后

> df -T 查看文件系统类型
```
如果使用xfs文件系统
xfs_growfs /dev/root_vg/root
如果使用ext4文件系统
resize2fs /dev/root_vg/root
```