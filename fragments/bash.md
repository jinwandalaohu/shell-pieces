1. 字符串拆分为数组
```bash
#!/bin/bash  
#Example for bash split string by another string  

str="WeLearnWelcomeLearnYouLearnOnLearnYiibai"  
delimiter=Learn  
s=$str$delimiter  
array=();  
while [[ $s ]];  
do  
  array+=( "${s%%"$delimiter"*}" );  
  s=${s#*"$delimiter"};  
done;  
declare -p array
```
> 在此bash脚本中，使用了以下参数扩展：
> - ${s%%"$delimiter"*} - 它删除最长的匹配后缀模式。
> - ${s#*"$delimiter"} - 它删除最短的匹配前缀模式。


