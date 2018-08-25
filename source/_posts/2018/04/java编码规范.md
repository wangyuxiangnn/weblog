---
title: java编码规范
categories: java 调优  
date: 2018-04-02 20:35:16
preview: https://isux.tencent.com/static/images/resources/banner-2.jpg
subtitle: 查看阿里的  《java开发手册》复制出来的  前人总结的经验   为了 告诫自己
---
1. 采用 4 个空格缩进，禁止使用 tab 字符。
2. if/for/while/switch/do 等保留字与括号之间都必须加空格。
3. 大括号的使用约定。如果是大括号内为空，则简洁地写成{}即可，不需要换行;如果 是非空代码块则:

        1) 左大括号前不换行。
        2) 左大括号后换行。
        3) 右大括号前换行。
        4) 右大括号后还有else等代码则不换行;表示终止的右大括号后必须换行。
        
    ```
    public static void main(String[] args) {
        // 缩进 4 个空格
        String say = "hello";
        // 运算符的左右必须有一个空格
        int flag = 0;
        // 关键词 if 与括号之间必须有一个空格，括号内的 f     与左括号，0 与右括号不需要空格 
        if (flag == 0) {
            System.out.println(say);
        }
        // 左大括号前加空格且不换行;左大括号后换行 if (flag == 1) {
            System.out.println("world");
        // 右大括号前换行，右大括号后有 else，不用换行 } else {
            System.out.println("ok");
        // 在右大括号后直接结束，则必须换行
        } 
    }
    ```
4. 注释的双斜线与注释内容之间有且仅有一个空格。
5. 方法参数在定义和传入时，多个参数逗号后边必须加空格。
 
        method("a", "b", "c");
6. 不同逻辑、不同语义、不同业务的代码之间插入一个空行分隔开来以提升可读性。
7. 定义 DO/DTO/VO 等 POJO 类时，不要设定任何属性默认值。
8. 构造方法里面禁止加入任何业务逻辑，如果有初始化逻辑，请放在 init 方法中。
9. 类内方法定义的顺序依次是:公有方法或保护方法 > 私有方法 > getter/setter方法。
10. 不要在 foreach 循环里进行元素的 remove/add 操作。remove 元素请使用 Iterator方式，如果并发操作，需要对 Iterator 对象加锁。
```
Iterator<String> iterator = list.iterator(); 
while (iterator.hasNext()) {
    String item = iterator.next(); 
    if (删除元素的条件) {
        iterator.remove();
    }
}
```
11. 使用 entrySet 遍历 Map 类集合 KV，而不是 keySet 方式进行遍历。说明:keySet 其实是遍历了 2 次，一次是转为 Iterator 对象，另一次是从 hashMap 中取出 key 所对应的 value。而 entrySet 只是遍历了一次就把 key 和 value 都放到了 entry 中，效 率更高。如果是 JDK8，使用 Map.foreach 方法。
12. 多线程并行处理定时任务时，Timer 运行多个 TimeTask 时，只要其中之一没有捕获抛出的异常，其它任务便会自动终止运行，使用 ScheduledExecutorService 则没有这个问题。
13. 在高并发场景中，避免使用”等于”判断作为中断或退出的条件。 说明:如果并发控制没有处理好，容易产生等值判断被“击穿”的情况，使用大于或小于的区间 判断条件来代替。
14. 很多 if 语句内的逻辑相当复杂，阅读者需要分析条件表达式的最终结果，才能明确什么 样的条件执行什么样的语句，那么，如果阅读者分析逻辑表达式错误呢?

    正例:

        final boolean existed = (file.open(fileName, "w") != null) && (...) || (...); 
        if (existed) {... }
    反例:

        if ((file.open(fileName, "w") != null) && (...) || (...)) { ...}
15. 谨慎注释掉代码。在上方详细说明，而不是简单地注释掉。如果无用，则删除。
16. 对 trace/debug/info级别的日志输出，必须使用条件输出形式或者使用占位符的方 式。

        if (logger.isDebugEnabled()) { //条件
        logger.debug("Processing trade with id: {} and symbol : {} ", id, symbol);  //占位符
        }
17. 后台输送给页面的变量必须加$!{var}——中间的感叹号。
18. 任何数据结构的构造或初始化，都应指定大小，避免数据结构无限增长吃光内存。
19. 对于暂时被注释掉，后续可能恢复使用的代码片断，在注释代码上方，统一规定使用三 个斜杠(///)来说明注释掉代码的理由。
20. 单元测试应该是全自动执行的，并且非交互式的。测试用例通常是被定期执行的，执行过程必须完全自动化才有意义。输出结果需要人工检查的测试不是一个好的单元测试。单元 测试中不准使用 System.out 来进行人肉验证，必须使用 assert 来验证。
21. 用户请求传入的任何参数必须做有效性验证。
22. 禁止向 HTML页面输出未经安全过滤或未正确转义的用户数据。
23. 库名与应用名称尽量一致。


        MySQL数据库


1. 表必备三字段:id, gmt_create, gmt_modified。 说明:其中id必为主键，类型为unsigned bigint、单表时自增、步长为1。gmt_create, gmt_modified 的类型均为 datetime 类型，前者现在时表示主动创建，后者过去分词表示被 动更新。
2. 表的命名最好是加上“业务名称_表的作用”。 正例:alipay_task / force_project / trade_config
3. 超过三个表禁止 join。需要 join 的字段，数据类型必须绝对一致;多表关联查询时， 保证被关联的字段需要有索引。
4. 建组合索引的时候，区分度最高的在最左边。存在非等号和等号混合判断条件时，在建索引时，请把等号条件的列前置。
5. 不要使用 count(列名)或 count(常量)来替代 count(*)，count(*)是 SQL92 定义的 标准统计行数的语法，跟数据库无关，跟 NULL 和非 NULL 无关。 说明:count(*)会统计值为 NULL 的行，而 count(列名)不会统计此列为 NULL 值的行。
6. 当某一列的值全是 NULL 时，count(col)的返回结果为 0，但 sum(col)的返回结果为 NULL，因此使用 sum()时需注意 NPE 问题。 正例:可以使用如下方式来避免sum的NPE问题:
    
        SELECT IF(ISNULL(SUM(g)),0,SUM(g)) FROM table;
7. 在代码中写分页查询逻辑时，若 count 为 0 应直接返回，避免执行后面的分页语句。
8. 禁止使用存储过程，存储过程难以调试和扩展，更没有移植性。
9. in 操作能避免则避免，若实在避免不了，需要仔细评估 in 后边的集合元素数量，控 制在 1000 个之内。
10. POJO 类的布尔属性不能加 is，而数据库字段必须加 is_，要求在 resultMap 中进行 字段与属性之间的映射。
11. 不要写一个大而全的数据更新接口。传入为 POJO 类，不管是不是自己的目标更新字
段，都进行 update table set c1=value1,c2=value2,c3=value3; 这是不对的。执行 SQL 时，不要更新无改动的字段，一是易出错;二是效率低;三是增加 binlog 存储。
12. @Transactional 事务不要滥用。事务会影响数据库的 QPS，另外使用事务的地方需 要考虑各方面的回滚方案，包括缓存回滚、搜索引擎回滚、消息补偿、统计修正等。