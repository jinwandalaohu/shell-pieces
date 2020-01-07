# git
> git tortoise 配置  
> 通过 
>  git config --global user.email "you@example.com"  
>  git config --global user.name "Your Name"
>  配置git全局用户名。



Git常用操作命令收集：

1）远程仓库相关命令
```

检出仓库：$ git clone git://github.com/jquery/jquery.git

查看远程仓库：$ git remote -v

添加远程仓库：$ git remote add [name] [url]

删除远程仓库：$ git remote rm [name]

修改远程仓库：$ git remote set-url --push[name][newUrl]

拉取远程仓库：$ git pull [remoteName] [localBranchName]

推送远程仓库：$ git push [remoteName] [localBranchName]
```


2）分支(branch)操作相关命令
```

查看本地分支：$ git branch

查看远程分支：$ git branch -r

查看所有分支: $ git branch -a ----红色为远程厂库

创建本地分支：$ git branch [name] ----注意新分支创建后不会自动切换为当前分支

切换分支：$ git checkout [name]

创建新分支并立即切换到新分支：$ git checkout -b [name]

删除分支：$ git branch -d [name] ----  -d选项只能删除已经参与了合并的分支，对于未有合并的分支是无法删除的。如果想强制删除一个分支，可以使用-D选项

合并分支：$ git merge [name] ----将名称为[name]的分支与当前分支合并

创建远程分支(本地分支push到远程)：$ git push origin [name]

删除远程分支：$ git push origin :heads/[name]
```


3）git fetch

![git fetch 与 git pull](../images/git_fetch.jpg)  
```bash
git fetch origin dev  # 拉取远程分支
```


 **git回退操作**
### git reset
```bash
# 修改完还未执行 git add (git checkout .和git add .是一对反义词)
git checkout .

# 使用git add 提交到暂存区，还未commit之前
git reset          # 先用Head指针覆盖当前的暂存区内容
git checkout .     # 再用暂存区内容覆盖工作区内容
或者
git reset --hard   # 直接使用head覆盖当前暂存区和工作区

# 已经git commit，还未git push
git reset --hard origin/master   # 从远程仓库把修改前代码取回来，然后覆盖本地仓库、本地暂存区和工作区  （提交要是用 git push -f 因为远程commit id 较新）
或者
git reset --hard last_commit_id  # 覆盖本地仓库、暂存区和工作区，其中查看last_commit_id命令为 git log
或者使用
git reset --mixed last_commit_id  # 覆盖本地的暂存区，
git checkout .                    # 覆盖本地工作区

# 已经git push
git reset --hard commit_id   # 修改错了，完全覆盖掉，使用
git reset    # 错误的把大文件添加到了缓存区，撤回添加
```
### 
git revert
> git reset --hard 撤销到某次提交 git revert 撤销某次提交  
> 撤销某一版本生成一个新的版本，比如，我们commit了三个版本（版本一、版本二、 版本三），突然发现版本二不行（如：有bug），想要撤销版本二，但又不想影响撤销版本三的提交，就可以用 git revert 命令来反做版本二，生成新的版本四，这个版本四里会保留版本三的东西，但撤销了版本二的东西。
1. git log 查看要回退的版本号。
2. 使用git revert -n 版本号， 通过 git commit -m 提交
3. git push 提交


> **git 提交时关于换行符的修改**
```bash
#提交检出均不转换
$ git config --global core.autocrlf false

#提交时转换为LF，检出时不转换
$ git config --global core.autocrlf input

#提交时转换为LF，检出时转换为CRLF
$ git config --global core.autocrlf true

```

> **Git在添加ignore文件之前就提交了项目无法再过滤问题**
```bash
# 为避免冲突先拉取最新代码
git pull

# 在本项目目录下清除缓存
git rm -r --cached .

# 修改 .gitignore 文件,重新添加
 git add .

# 重新提交
git commit -m ""

# 文件夹上还有红色感叹号
#通过删除文件夹再还原（通过右键 git clean ）
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