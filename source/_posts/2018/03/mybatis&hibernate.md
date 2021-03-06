---
title: Mybatis与Hibernate
tags: 面试
categories: 框架知识
date: 2018-03-19 20:35:16
preview: http://upload-images.jianshu.io/upload_images/12906348-3e6b62e5b503ca3c?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240
subtitle: 生命力的意义在于拚搏，因为世界本身就是一个竞技场，就是无数次被礁石击碎又无数闪地扑向礁石，生命的绿荫才会越长越茂盛。
---
```
  Mybatis与Hibernate
```
mybatis官网 [github地址](http://www.mybatis.org/mybatis-3/zh/dynamic-sql.html "mybatis")

1. hibernate是全自动，而mybatis是半自动
hibernate完全可以通过对象关系模型实现对数据库的操作，拥有完整的JavaBean对象与数据库的映射结构来自动生成sql。而mybatis仅有基本的字段映射，对象数据以及对象实际关系仍然需要通过手写sql来实现和管理。

2. hibernate数据库移植性远大于mybatis
hibernate通过它强大的映射结构和hql语言，大大降低了对象与数据库(oracle、mysql等)的耦合性，而mybatis由于需要手写sql，因此与数据库的耦合性直接取决于程序员写sql的方法，如果sql不具通用性而用了很多某数据库特性的sql语句的话，移植性也会随之降低很多，成本很高。

3. hibernate拥有完整的日志系统，mybatis则欠缺一些
hibernate日志系统非常健全，涉及广泛，包括：sql记录、关系异常、优化警告、缓存提示、脏数据警告等;而mybatis则除了基本记录功能外，功能薄弱很多。

4. mybatis相比hibernate需要关心很多细节
hibernate配置要比mybatis复杂的多，学习成本也比mybatis高。但也正因为mybatis使用简单，才导致它要比hibernate关心很多技术细节。mybatis由于不用考虑很多细节，开发模式上与传统jdbc区别很小，因此很容易上手并开发项目，但忽略细节会导致项目前期bug较多，因而开发出相对稳定的软件很慢，而开发出软件却很快。hibernate则正好与之相反。但是如果使用hibernate很熟练的话，实际上开发效率丝毫不差于甚至超越mybatis。

5. sql直接优化上，mybatis要比hibernate方便很多
由于mybatis的sql都是写在xml里，因此优化sql比hibernate方便很多。而hibernate的sql很多都是自动生成的，无法直接维护sql;虽有hql，但功能还是不及sql强大，见到报表等变态需求时，hql也歇菜，也就是说hql是有局限的;hibernate虽然也支持原生sql，但开发模式上却与orm不同，需要转换思维，因此使用上不是非常方便。总之写sql的灵活度上hibernate不及mybatis。
随着使用情况的不断增多，我又做了进一步的总结总结：
    mybatis：小巧、方便、高效、简单、直接、半自动
    hibernate：强大、方便、高效、复杂、绕弯子、全自动

mybatis：

1. 入门简单，即学即用，提供了数据库查询的自动对象绑定功能，而且延续了很好的SQL使用经验，对于没有那么高的对象模型要求的项目来说，相当完美。

2. 可以进行更为细致的SQL优化，可以减少查询字段。

3. 缺点就是框架还是比较简陋，功能尚有缺失，虽然简化了数据绑定代码，但是整个底层数据库查询实际还是要自己写的，工作量也比较大，而且不太容易适应快速数据库修改。

4. 二级缓存机制不佳。

hibernate：

1. 功能强大，数据库无关性好，O/R映射能力强，如果你对Hibernate相当精通，而且对Hibernate进行了适当的封装，那么你的项目整个持久层代码会相当简单，需要写的代码很少，开发速度很快，非常爽。

2. 有更好的二级缓存机制，可以使用第三方缓存。

3. 缺点就是学习门槛不低，要精通门槛更高，而且怎么设计O/R映射，在性能和对象模型之间如何权衡取得平衡，以及怎样用好Hibernate方面需要你的经验和能力都很强才行。

举个形象的比喻：

    mybatis：机械工具，使用方便，拿来就用，但工作还是要自己来作，不过工具是活的，怎么使由我决定。

    hibernate：智能机器人，但研发它(学习、熟练度)的成本很高，工作都可以摆脱他了，但仅限于它能做的事。