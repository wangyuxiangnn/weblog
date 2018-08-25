---
title: Mybatis查询缓存引起的问题
categories: java
date: 2018-08-08 19:35:16
preview: https://images.pexels.com/photos/1302940/pexels-photo-1302940.jpeg?auto=compress&cs=tinysrgb&dpr=2&h=750&w=1260
subtitle: 年少的承诺，执着的相守。看似美好，却是无情。
---

  - Mybatis在查询时会采用缓存机制，分为一级缓存和二级缓存，一级缓存默认就会开启，二级缓存需要配置才可以使用。
  - 一级缓存基于 PerpetualCache 的 HashMap 本地缓存，其存储作用域为 Session，当 Session flush 或 close 之后，该Session中的所有 Cache 就将清空。
  - 二级缓存与一级缓存其机制相同，默认也是采用 PerpetualCache，HashMap存储，不同在于其存储作用域为 Mapper(Namespace)，并且可自定义存储源，如 Ehcache、Hazelcast等。
  - 对于缓存数据更新机制，当某一个作用域(一级缓存Session/二级缓存Namespaces)的进行了 C/U/D 操作后，默认该作用域下所有 select 中的缓存将被clear。
  - 在缓存的作用域中，如果调用的方法的参数一样，就会使用缓存中查询的结果，而不会再次查询数据库。

问题:
    ```
    此处为代码
    public void updateXModel(XAbout xAbout){
        XModel xModelAgo=xModelMapper.selectByPrimaryKey(xAbout.getXModelId());
        if(xModelAgo != null && !xModel.getPhone.equals(xAbout.getPhone)){
            xModelAgo.setPhone(xAbout.getPhone);   //问题做法
            updatePhone(xModelAgo);
            // updatePhone(new XModel(xAbout.getXModelId(),xAbout.getPhone,null,null,null)); // 正确做法
        }
    }
    public void updatePhone(XModel xModel){
        XModel xModelAgo=xModelMapper.selectByPrimaryKey(xModel.getId());
        if(xModelAgo != null && xModelAgo.getPhone.equals(xModel.getPhone)){
            //奇葩的是竟然相等。。。
        }
    }
    ```
### 定位问题:
   ##### mybatis 在查询数据库，返回一个结果，当修改当前结果后，再次执行这个查询方法,返回的是修改后的结果。
   
   **在遇到比较复杂的业务逻辑的时候，比如需要迭代一个方法，获取一个树形结构的数据，此时由于方法会被多次调用，对数据库就会进行多次相同的查询，从第二次调用方法之后就会使用一级缓存。如果你在方法中对查询出来的结果对象直接进行了修改，比如修改了某个属性的值，在下一次调用方法是进行这个查询时，返回的结果是你修改过的结果对象，而不再是从数据库中查询出来的值了，这样可能就和预期的处理不同了，容易造成逻辑错误。因此，应该避免直接操作数据库查询获取的对象。**

###  解决办法
    当作为参数传入另一个方法做逻辑处理，尽量避免直接操作数据库查询获取的对象;
    应该 重新 new XModel(null,null,null,null,null)  ok!

### 美图:
![image](https://images.pexels.com/photos/318378/pexels-photo-318378.jpeg?auto=compress&cs=tinysrgb&dpr=2&h=750&w=1260)

### 总结:
- 这样寡淡贫瘠的一生，遇到你却轰轰烈烈到不像自己。
