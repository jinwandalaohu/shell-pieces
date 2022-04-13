### fdisk 进行分区 （分区完成后使用 partprobe 使分区生效）
#### lvm分区划分
> fdisk 分区后通过 t 命令修改id为8e
> pvcreate /dev/vda1  
> vgcreate      
> lvcreate  `lvcreate -L 200G -n lvName vgName`
> mkfs.ext4  mkfs.xfs 进行格式化。
> lsblk -f 查看uuid
> vi /etc/fstab 修改启动挂载，  option(defaults)   0 (不dump) 0 (跳过fsck)   

### 

### 扩容后
- lvextend -L 200G|+100G /lv/name
> df -Th 查看文件系统类型，还需要刷新文件系统大小。  
```bash
#如果使用xfs文件系统
xfs_growfs /dev/root_vg/root
#如果使用ext4文件系统
resize2fs /dev/root_vg/root
```