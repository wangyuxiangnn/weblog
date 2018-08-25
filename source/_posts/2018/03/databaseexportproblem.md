---
title: shell 脚本中 执行mysql查询语句并将结果导出excel中，表内容与表头别名中文乱码问题
date: 2018-03-08 19:35:16
preview: https://images.pexels.com/photos/1261415/pexels-photo-1261415.jpeg?auto=compress&cs=tinysrgb&dpr=2&h=750&w=1260
subtitle: mysql 导出select语句结果到excel文件，中文乱码问题，语句中将表头重新命名为中文导出到excel乱码
---
## 解决表内容乱码 ## 
在执行mysql查询语句前指定编码
==--default-character-set gbk==  


```
mysql -N -hlocalhost -uroot -p123456 --default-character-set gbk tms_ems_test2 -e "select cchannelName from channel;"> channel.xls  #-N去掉表头 按列展示
```


## 解决在shell脚本中执行sql语句表头别名乱码问题

```
mysql -hlocalhost -uroot -p123456 tms_ems_test2 -e "select cchannelName as '频道名称' from channel;"> channel.xls 

iconv -f utf8 -t gbk channel.xls -o channel.xls  #将utf8转为gbk
```
