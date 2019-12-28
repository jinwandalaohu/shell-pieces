# git
> git tortoise 配置  
> 通过 
>  git config --global user.email "you@example.com"
>  git config --global user.name "Your Name"
> 配置git全局用户名。


```
Git常用操作命令收集：

1) 远程仓库相关命令

检出仓库：$ git clone git://github.com/jquery/jquery.git

查看远程仓库：$ git remote -v

添加远程仓库：$ git remote add [name] [url]

删除远程仓库：$ git remote rm [name]

修改远程仓库：$ git remote set-url --push[name][newUrl]

拉取远程仓库：$ git pull [remoteName] [localBranchName]

推送远程仓库：$ git push [remoteName] [localBranchName]

2）分支(branch)操作相关命令

查看本地分支：$ git branch

查看远程分支：$ git branch -r

查看所有分支: $ git branch -a ----红色为远程厂库

创建本地分支：$ git branch [name] ----注意新分支创建后不会自动切换为当前分支

切换分支：$ git checkout [name]

创建新分支并立即切换到新分支：$ git checkout -b [name]

删除分支：$ git branch -d [name] ---- -d选项只能删除已经参与了合并的分支，对于未有合并的分支是无法删除的。如果想强制删除一个分支，可以使用-D选项

合并分支：$ git merge [name] ----将名称为[name]的分支与当前分支合并

设置默认推送的远程地址: $ git push -u git master

创建远程分支(本地分支push到远程)：$ git push origin [name]

删除远程分支：$ git push origin :heads/[name]

3)版本(tag)操作相关命令

查看版本：$ git tag

创建版本：$ git tag [name]

删除版本：$ git tag -d [name]

查看远程版本：$ git tag -r

创建远程版本(本地版本push到远程)：$ git push origin [name]

删除远程版本：$ git push origin :refs/tags/[name]
4) 子模块(submodule)相关操作命令

添加子模块：$ git submodule add [url] [path]

如：$ git submodule add git://github.com/soberh/ui-libs.git src/main/webapp/ui-libs

初始化子模块：$ git submodule init ----只在首次检出仓库时运行一次就行

更新子模块：$ git submodule update ----每次更新或切换分支后都需要运行一下

删除子模块：（分4步走哦）

1)$ git rm --cached [path]

2) 编辑“.gitmodules”文件，将子模块的相关配置节点删除掉

3) 编辑“.git/config”文件，将子模块的相关配置节点删除掉

4) 手动删除子模块残留的目录

5）忽略一些文件、文件夹不提交
```
## usecase
* 新建一个厂库关联的git
```
   本地git init 新建一个厂库，github上新建一个厂库，本地 git remote add github URl 添加远程厂库，git pull报错：   
   git pull 失败 ,提示：fatal: refusing to merge unrelated histories  
   其实这个问题是因为 两个 根本不相干的 git 库， 一个是本地库， 一个是远端库， 然后本地要去推送到远端， 远端觉得这个本地库跟自己不相干， 所以告知无法合并。
   git pull github master --allow-unrelated-histories
   后面加上 --allow-unrelated-histories ， 把两段不相干的 分支进行强行合并
   然后再push就可以了。
```
* git 三板斧
```
git add .
git status
git commit -m "comments"
git push
```
* 修改文件夹名称
```
git mv -f oldfolder newfolder
git add -u newfolder (-u选项会更新已经追踪的文件和文件夹)
git commit -m "changed the foldername whaddup"
```