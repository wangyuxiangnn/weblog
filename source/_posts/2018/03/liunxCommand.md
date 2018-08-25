---
title: liunx传输命令 
categories: liunx
date: 2018-03-13 20:35:16
preview: https://images.pexels.com/photos/119404/pexels-photo-119404.jpeg?auto=compress&cs=tinysrgb&dpr=2&h=750&w=1260
subtitle: rsync代替scp命令远程传输大文件，支持断点断续
---
## liunx同步命令(断点断续):

```
rsync -P Documents/IWorkSpace/SendMail/out/artifacts/SendMail_jar/SendMail.jar testRetailer:~
```

连接国外网络不好，使用rsync代替scp命令远程传输大文件

最近到国外的网络环境很差，丢包率大的感人，还时不时地断开，这时候如果要在本机和远程服务器间使用scp命令传输大文件的话，成功与否只能看运气了。传输过程中一个不小心断开了，只好从头再来一遍。其实对于大文件的传输，我们可以使用rsync来代替scp命令。

连接国外网络不好，使用rsync代替scp命令远程传输大文件

rsync主要是在类unix系统下作为数据镜像备份和文件同步工具使用的，从软件的命名上就可以看出来了——remote sync。

它的特性如下：

可以镜像保存整个目录树和文件系统。
可以很容易做到保持原来文件的权限、时间、软硬链接等等。
无须特殊权限即可安装。
优化的流程，文件传输效率高。
可以使用rcp、ssh等方式来传输文件，当然也可以通过直接的socket连接。
支持匿名传输。
这里我们只用它能够断点续传的特点在网络不好的环境下传输大的文件，算是有点大材小用了。就传输单个文件来说，它的用法和scp命令差不多，比如我要把远程服务器linode-server上的数据库备份文件database-backup.sql保存到本地。

命令形式如下：
```
xxx@xxx:~$ rsync -P user@ip:/home/daweibro/database-backup.sql /home/daweibro/.
```

daweobro@linode-server's password:
database-backup.sql
     34,948,241 100%   96.58kB/s    0:05:53 (xfr#1, to-chk=0/1)
rsync默认使用ssh的22端口，那么如果我们的服务器为了安全已经修改成其他的端口，比如端口是1234那怎么办呢？可以加上 -e 'ssh -p 1234'参数来指定端口号：

```
rsync -P -e 'ssh -p 1234' user@ip:/home/daweibro/database-backup.sql /home/daweibro/.
```

