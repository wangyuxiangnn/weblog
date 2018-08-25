---
title: 用户昵称允许设置😄️
categories: java 
date: 2018-08-06 20:35:16
preview: https://images.pexels.com/photos/379964/pexels-photo-379964.jpeg?auto=compress&cs=tinysrgb&dpr=2&h=750&w=1260
subtitle: 我要开花，是为了完成作为一株花的庄严生命，不管你们怎么看我，我都要开花！——林清玄《百合花开》
---

utf8mb4兼容utf8，且比utf8能表示更多的字符。
看unicode编码区，从1 ～ 126就属于传统utf8区，当然utf8mb4也兼容这个区，126行以下就是utf8mb4扩充区，什么时候你需要存储那些字符，你才用utf8mb4,否则只是浪费空间。

问题:
- 用户修改名称时，输入了表情，后台报错

```java
### Error updating database.  Cause: java.sql.SQLException: Incorrect string value: '\xF0\x9F\x99\x83\xF0\x9F...' for column 'title' at row 1
### The error may involve com.insertSelective-Inline
### The error occurred while setting parameters
### SQL: insert into qww  ( id, uid, title ) values ( ?,  ?,  ? )
### Cause: java.sql.SQLException: Incorrect string value: '\xF0\x9F\x99\x83\xF0\x9F...' for column 'title' at row 1
; uncategorized SQLException; SQL state [HY000]; error code [1366]; Incorrect string value: '\xF0\x9F\x99\x83\xF0\x9F...' for column 'title' at row 1; nested exception is java.sql.SQLException: Incorrect string value: '\xF0\x9F\x99\x83\xF0\x9F...' for column 'title' at row 1
```

### 定位问题:
Emoji表情符号为4个字节的字符，而 utf8 字符集只支持1-3个字节的字符，导致无法写入数据库。
**注**: MySQL的版本不能太低，低于5.5.3的版本不支持utf8mb4编码。

![image](https://upload-images.jianshu.io/upload_images/12906348-535ed6a9e8f05197.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

###  解决办法
##### 如果数据库默认编码为 utf8mb4，则不需要修改mysql表字符集，例如
```
CREATE TABLE `my_table` (
  `id` int(11) NOT NULL AUTO_INCREMENT,
  `create_time` datetime DEFAULT CURRENT_TIMESTAMP,
  `update_time` datetime DEFAULT CURRENT_TIMESTAMP ON UPDATE CURRENT_TIMESTAMP,
  `is_delete` tinyint(4) DEFAULT '0',
  PRIMARY KEY (`id`)
) ENGINE=InnoDB AUTO_INCREMENT=3 DEFAULT CHARSET=utf8mb4 COMMENT='备注';
```

##### 如果数据库默认编码不是utf8mb4 则需要修改MySQL数据库字符集
```
# 修改数据库:
ALTER DATABASE database_name CHARACTER SET = utf8mb4 COLLATE = utf8mb4_unicode_ci;
# 修改表:
ALTER TABLE table_name CONVERT TO CHARACTER SET utf8mb4 COLLATE utf8mb4_unicode_ci;
# 修改表字段:
ALTER TABLE table_name CHANGE column_name column_name VARCHAR(191) CHARACTER SET utf8mb4 COLLATE utf8mb4_unicode_ci;
```

##### java 配置druid数据库链接池
```
@Bean(name = "dataSource")
    @Primary
    public DataSource dataSource() {
        DruidDataSource dataSource = new DruidDataSource();
        dataSource.setDriverClassName(driverClass);
        dataSource.setUrl(url);
        dataSource.setUsername(user);
        dataSource.setPassword(password);
        dataSource.setConnectionInitSqls(Arrays.asList("set names utf8mb4")); #须加入此行,获取mysql连接时，指定编码为utf8mb4
        //configuration
        dataSource.setInitialSize(initialSize);
        dataSource.setMinIdle(minIdle);
        dataSource.setMaxActive(100);
        dataSource.setMaxWait(maxWait);
        dataSource.setTimeBetweenEvictionRunsMillis(timeBetweenEvictionRunsMillis);
        dataSource.setValidationQuery(validationQuery);
        dataSource.setTestWhileIdle(testWhileIdle);
        dataSource.setTestOnBorrow(testOnBorrow);
        return dataSource;
    }
```
### 总结:
- hahas
