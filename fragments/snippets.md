1. 移除文件的前几行与后几行  
> echo "select tablename from pg_tables where schemaname='public'" | psql -p 25433 -d gsxt_flb -U postgres -q |tail -n +3 |head -n -2
2. 
3. 