# conda 

> python 包管理工具

```bash
conda info -e
# 查看已安装的环境
conda list
# 查看当前环境的已安装的包 

conda create --name python36 python=3.6
# 创建一个新的python环境

# 激活某个环境
activate python36 # for Windows
source activate python36 # for Linux & Mac

deactivate python36 # for Windows
source deactivate python36 # for Linux & Mac

# 删除一个已有的环境
conda remove --name python36 --all

conda list -n python36
# 查看某个指定环境已安装的包

conda create -n python2 --clone py2
conda remove -n py2 --all
# 重命名

conda search numpy
# 查找package信息
conda install -n python34 numpy 
# 如果不用-n指定环境名称，则被安装在当前活跃环境 也可以通过-c指定通过某个channel安装(软件来源)

 ```

 * 更新package
 ```bash
# 更新package
conda update -n python34 numpy

# 删除package
conda remove -n python34 numpy

# 更新conda，保持conda最新
conda update -n base conda

# 更新anaconda
conda update anaconda

# 更新python
conda update python
 ```

 * pip 
> 包管理工具

