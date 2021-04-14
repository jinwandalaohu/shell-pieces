
## 下载镜像源到本地。
### 通过repo文件配置离线yum源
1. 下载对应的repo文件配置为enable。
2. 配置成功后通过 <code >yum repolist</code> 命令列出可用yum 源。
3. 通过 <code>reposync -r [base yum源名称 列出的第二项]</code> 
4. <code>createrepo -v</code>  # 在下载的yum源文件中的Packages文件夹中执行，报错移除软件包
5. yum clean all
6. yum makecache fast
   
### 通过下载rpm配置离线yum预源
> yum deplist ansible  命令可以列出安装ansible时所需要的依赖。  
> yum install --downloadonly --downloaddir=/home/yumRepo/ansible/ ansible   下载ansible的所有依赖  
> yum install --downloadonly --downloaddir=/home/createrepo createrepo       下载创建厂库的依赖  
> 将下载的rmp包进行拷贝到局域网机器上   
> 通过rpm -ivh 安装createrepo  
> 创建/etc/yum.d/ansible.repo  
>> [ansible]  
name=ansible
baseurl=file:///home/yumRepo/ansible # 配置本地目录作为源
gpgcheck=0                           # 关闭
enabled=1                            # 使用当前源
gpgkey=file:///etc/pki/rpm-gpg/RPM-GPG-KEY-Centos-7
> createrepo -d /home/yumRepo/ansible  
> yum repolist  #查看已经安装的源
> yum clean all 
> yum makecache  
> yum install ansible -y