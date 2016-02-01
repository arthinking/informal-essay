# SQL语句优化
## IN语句的优化    
### demo 1
慢语句  
```SQL
SELECT * FROM t_user WHERE id IN(SELECT MAX(id) FROM t_user GROUP BY country)
```  
优化
```SQL
SELECT * FROM t_user t1, (SELECT MAX(id) AS id FROM t_user GROUP BY country) t2 WHERE t1.id = t2.id
```  

## 大表查询优化
随着数据库数据的增加，查询速度会越来越慢，可以开启log_slow_queries慢查询日志，通过查看[慢查询日志](http://www.itzhai.com/access-speed-too-slow-mysql-statement-analysis.html "慢查询日志")找到慢语句，进行优化。  


# 问题
## 数据库中主键设计原则
