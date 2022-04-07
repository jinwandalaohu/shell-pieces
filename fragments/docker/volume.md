## 存储方式
![存储的挂载方式](../../images/types-of-mounts.png)
> 文件的持久化方式， 一般来说volume是最好的方式。  
> volume通过docker volume create 创建，也可以在创建容器或服务时创建，这种方式。  
> bind mount 通过指定主机上的绝对路径创建，通过这种方式容器可以修改主机的配置或使用主机的文件（如maven仓库）。  
> tmpfs mount 不会持久化到硬盘上.  
> **bind mount 容器内原有的东西不会映射出来，也就是说映射后的文件夹总为空， volume可以将容器内的文件映射出来。**   
## 相关说明
1. volume 可以在多个容器间共享。  
2. 通过 volume drivers 可以设置使用远程的主机或云，来做加密或其他操作。
3. 新卷的内容可以由容器预填充。  
4. 