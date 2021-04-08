### maven 安装本地jar包。
> 注意添加单引号
```shell
mvn -e install:install-file -Dfile='F:\架包\ojdbc6-11.2.0.jar' -DgroupId='com.oracle' -DartifactId=ojdbc6 -Dversion='11.2.0' -Dpackaging=jar
```
