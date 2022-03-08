### pip 离线安装 wheel文件
>> 在本地通过conda创建一个新的与生产一致的环境进行操作。  
1. 在有网的环境安装whl文件
```shell
    pip install polemarch-1.8.0-py3-none-any.whl
```
2. pip list查看安装的包,将包名称写入本地
```shell
    pip freeze >requirements.txt
```
3. 下载依赖的包
```shell
    pip  download -r requirements.txt -d /local/path
```
4. 在内网环境中安装
```shell
    pip install --no-index -f /local/path -r requirements.txt

```
