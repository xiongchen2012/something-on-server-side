##### PL/SQL或SQLDeveloper执行以下SQL语句查看剩余量  
---  
``` sql
SELECT a.tablespace_name, 
       a.bytes total, 
       b.bytes used, 
       c.bytes free, 
       (b.bytes * 100) / a.bytes "% USED ", 
       (c.bytes * 100) / a.bytes "% FREE " 
FROM 
       sys.sm$ts_avail a, sys.sm$ts_used b, sys.sm$ts_free c 
WHERE 
       a.tablespace_name = b.tablespace_name 
AND    a.tablespace_name = c.tablespace_name;   
```  

##### 剩余空间低于10%的表空间有必要进行扩容  
---  
``` sql
--连接到数据库(sqlplus / as sysdba)
ALTER TABLESPACE USERS ADD DATAFILE '/var/oradata\user001.dbf' SIZE 32G;
```
