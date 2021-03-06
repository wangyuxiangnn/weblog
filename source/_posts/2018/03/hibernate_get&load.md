---
title: Hibernate中get和load方法的区别
tags: 面试
categories: 框架知识
date: 2018-03-03 19:00:16
preview: https://sandbox.runjs.cn/uploads/rs/247/l96nyyh9/hibernate-logo.svg
subtitle: Hibernate中get和load方法的区别
---
1. 对于get方法，hibernate会确认一下该id对应的数据是否存在，首先在session缓存中查找，然后在二级缓存中查找，还没有就查询数据库，数据库中没有就返回null。这个相对比较简单，也没有太大的争议。主要要说明的一点就是在这个版本中get方法也会查找二级缓存！

2.  load方法加载实体对象的时候，根据映射文件上类级别的lazy属性的配置(默认为true)，分情况讨论：
    (1)若为true,则首先在Session缓存中查找，看看该id对应的对象是否存在，不存在则使用延迟加载，返回实体的代理类对象(该代理类为实体类的子类，由CGLIB动态生成)。等到具体使用该对象(除获取OID以外)的时候，再查询二级缓存和数据库，若仍没发现符合条件的记录，则会抛出一个ObjectNotFoundException。
    (2)若为false,就跟get方法查找顺序一样，只是最终若没发现符合条件的记录，则会抛出一个ObjectNotFoundException。

 

这里get和load有两个重要区别:

    如果未能发现符合条件的记录，get方法返回null，而load方法会抛出一个ObjectNotFoundException。
    load方法可返回没有加载实体数据的代理类实例，而get方法永远返回有实体数据的对象。(对于load和get方法返回类型：好多书中都说：“get方法永远只返回实体类”，实际上并不正确，get方法如果在session缓存中找到了该id对应的对象，如果刚好该对象前面是被代理过的，如被load方法使用过，或者被其他关联对象延迟加载过，那么返回的还是原先的代理对象，而不是实体类对象，如果该代理对象还没有加载实体数据（就是id以外的其他属性数据），那么它会查询二级缓存或者数据库来加载数据，但是返回的还是代理对象，只不过已经加载了实体数据。)
 

总之对于get和load的根本区别，一句话
      
<font color=#0099ff size=4 face="黑体">hibernate对于load方法认为该数据在数据库中一定存在，可以放心的使用代理来延迟加载，如果在使用过程中发现了问题，只能抛异常；而对于get方法，hibernate一定要获取到真实的数据，否则返回null。</font>      
