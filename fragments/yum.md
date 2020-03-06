
## 下载镜像源到本地。
1. 下载对应的repo文件配置为enable。
2. 配置成功后通过 <code >yum repolist</code> 命令列出可用yum 源。
3. 通过 <code>reposync -r [base yum源名称 列出的第二项]</code> 
4. <code>createrepo -v</code>  # 在下载的yum源文件中的Packages文件夹中执行，报错移除软件包
5. yum clean all
6. yum makecache fast