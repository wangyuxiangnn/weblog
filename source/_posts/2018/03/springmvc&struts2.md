---
title: SpringMVC与struts2的区别
tags: 面试
categories: 框架知识
date: 2018-03-05 18:35:16
preview: https://images.pexels.com/photos/365341/pexels-photo-365341.jpeg?auto=compress&cs=tinysrgb&h=750&w=1260
subtitle: You can talk to that author you love, ask him anything you want.
---
    然后简单说一下Spring MVC与Spring的关系。Spring可以说是一个管理bean的容器，也可以说是包括很多开源项目的总称，而Spring MVC是其中一个开源项目。如果简单进行一个流程，当http请求一到，由容器（如：Tomcat）解析http形成一个request，通过映射关系（比如路径，方法，参数）被Spring MVC一个分发器去找到可以处理这个请求的bean，在Tomcat里面就由Spring管理bean的一个池子（bean容器）里面找到。处理完了就把响应返回。

1. Struts2是类级别的拦截， 一个类对应一个request上下文，SpringMVC是方法级别的拦截，一个方法对应一个request上下文，而方法同时又跟一个url对应,所以说从架构本身上SpringMVC就容易实现restful url,而struts2的架构实现起来要费劲，因为Struts2中Action的一个方法可以对应一个url，而其类属性却被所有方法共享，这也就无法用注解或其他方式标识其所属方法了。
2. 由上边原因，SpringMVC的方法之间基本上独立的，独享request response数据，请求数据通过参数获取，处理结果通过ModelMap交回给框架，方法之间不共享变量，而Struts2搞的就比较乱，虽然方法之间也是独立的，但其所有Action变量是共享的，这不会影响程序运行，却给我们编码 读程序时带来麻烦，每次来了请求就创建一个Action，一个Action对象对应一个request上下文。
3. 由于Struts2需要针对每个request进行封装，把request，session等servlet生命周期的变量封装成一个一个Map，供给每个Action使用，并保证线程安全，所以在原则上，是比较耗费内存的。
4. 拦截器实现机制上，Struts2有以自己的interceptor机制，SpringMVC用的是独立的AOP方式，这样导致Struts2的配置文件量还是比SpringMVC大。
5. SpringMVC的入口是servlet，而Struts2是filter（这里要指出，filter和servlet是不同的。以前认为filter是servlet的一种特殊），这就导致了二者的机制不同，这里就牵涉到servlet和filter的区别了。
6. SpringMVC集成了Ajax，使用非常方便，只需一个注解@ResponseBody就可以实现，然后直接返回响应文本即可，而Struts2拦截器集成了Ajax，在Action中处理时一般必须安装插件或者自己写代码集成进去，使用起来也相对不方便。
7. SpringMVC验证支持JSR303，处理起来相对更加灵活方便，而Struts2验证比较繁琐，感觉太烦乱。
8. Spring MVC和Spring是无缝的。从这个项目的管理和安全上也比Struts2高（当然Struts2也可以通过不同的目录结构和相关配置做到SpringMVC一样的效果，但是需要xml配置的地方不少）。
9. 设计思想上，Struts2更加符合OOP的编程思想， SpringMVC就比较谨慎，在servlet上扩展。
10. SpringMVC开发效率和性能高于Struts2。
11. SpringMVC可以认为已经100%零配置。

Spring MVC 执行流程

![](https://upload-images.jianshu.io/upload_images/3676154-7975adeae000fc84.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/700)
##### Spring MVC 执行流程如下，共包括八步：
        
        Spring MVC 相关接口解释：
        1）DispatcherServlet
        前端控制器，所有的请求都有经过它来统一分发，请求会被分发给对应的 Handler。
        2）HandlerMapping（处理器映射器）
        解析请求链接，然后根据请求链接找到执行这个请求的类（HandlerMapping 所说的 handler）。
        3）HandlerAdapter（处理器适配器）
        调用具体的方法对用户发来的请求来进行处理。
        4）Controller
        Controller 将处理用户请求，Controller 处理完用户请求，则返回 ModelAndView 对象给 DispatcherServlet 前端控制器。
        从宏观角度考虑，DispatcherServlet 是整个 Web 应用的控制器；从微观考虑，Controller 是单个 Http 请求处理过程中的控制器。
        5）ViewResolver（视图解析器）
        解析 MdoelAndView，将 MdoelAndView 中的逻辑视图名变为一个真正的 View 对象，并将 MdoelAndView 中的 Model 取出。
