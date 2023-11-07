### testping.sh

```shell
#!/bin/bash
dir=$(cd $(dirname $0);pwd)
while read line;
do
	[[ $line =~ ^[[:blank:]]*# ]] && continue
	[[ -z $line ]] && continue
	result=$(echo -e '\x1d' |telnet $line 2>/dev/null |grep 'Escape character' | wc -l)
if [[${result} -lt 1]];then
    echo $line no
else
    echo $line yes
fi
# sleep 0.5
done < $dir/ipfile.txt
```

### ipfile.txt

#北京平台库
10.90.130.189 1521
10.90.130.190 1521
10.90.130.191 1521
10.90.130.192 1521
10.90.130.193 1521

#北京业务库
10.90.130.83 1521
10.90.130.84 1521
10.90.130.85 1521
10.90.130.86 1521
10.90.130.87 1521

#上海平台库
10.110.32.53 11521
10.110.32.54 11521
10.110.32.55 11521
10.110.32.56 11521
10.110.32.57 11521

#上海业务库
10.110.32.48 11521
10.110.32.49 11521
10.110.32.50 11521
10.110.32.51 11521
10.110.32.52 11521
