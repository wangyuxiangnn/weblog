---
title: 项目配置运行
preview: https://images.pexels.com/photos/1260591/pexels-photo-1260591.jpeg?auto=compress&cs=tinysrgb&dpr=2&h=750&w=1260
date: 2018-03-26 20:35:16
subtitle: VM options： -Dspring.profiles.active=personal_dev传给spring加载指定：类注解@Profile("personal_dev")  的类
---

## bug
```
bug 1: 三月 14, 2018 9:39:36 下午 org.apache.catalina.core.StandardContext loadOnStartup
严重: Servlet [ApiInitializer] in web application [] threw load() exception
org.springframework.beans.factory.NoSuchBeanDefinitionException: No qualifying bean of type 'com.technow.bbclip.common.logic.component.SiteComponent' available: expected at least 1 bean which qualifies as autowire candidate. Dependency annotations: {@org.springframework.beans.factory.annotation.Autowired(required=true)}
	at org.springframework.beans.factory.support.DefaultListableBeanFactory.raiseNoMatchingBeanFound(DefaultListableBeanFactory.java:1493)
	at org.springframework.beans.factory.support.DefaultListableBeanFactory.doResolveDependency(DefaultListableBeanFactory.java:1104)
	at org.springframework.beans.factory.support.DefaultListableBeanFactory.resolveDependency(DefaultListableBeanFactory.java:1066)
	at org.springframework.beans.factory.annotation.AutowiredAnnotationBeanPostProcessor$AutowiredFieldElement.inject(AutowiredAnnotationBeanPostProcessor.java:585)
	at org.springframework.beans.factory.annotation.InjectionMetadata.inject(InjectionMetadata.java:88)
	at org.springframework.beans.factory.annotation.AutowiredAnnotationBeanPostProcessor.postProcessPropertyValues(AutowiredAnnotationBeanPostProcessor.java:366)
	at org.springframework.beans.factory.support.AbstractAutowireCapableBeanFactory.populateBean(AbstractAutowireCapableBeanFactory.java:1264)
	at org.springframework.beans.factory.support.AbstractAutowireCapableBeanFactory.doCreateBean(AbstractAutowireCapableBeanFactory.java:553)
	at org.springframework.beans.factory.support.AbstractAutowireCapableBeanFactory.createBean(AbstractAutowireCapableBeanFactory.java:483)
	at org.springframework.beans.factory.support.AbstractBeanFactory$1.getObject(AbstractBeanFactory.java:306)
	at org.springframework.beans.factory.support.DefaultSingletonBeanRegistry.getSingleton(DefaultSingletonBeanRegistry.java:230)

**报错原因**：由于没有加载成功spring容器，导致注入时没有重spring容器中找到被注入对象。
**寻找问题**：查看spring配置发现 @Profile 注解，只有激活此注解才会执行
**解决方法**:  ``作为JVM的系统属性,传入-Dspring.profiles.active=值，激活带有@Profile（值)``

环境与Profile
1. 原因：
在开发阶段，某些环境相关的配置并不适合直接迁移到生产环境中。比如数据库配置、加密算法以及与外部系统的集成是跨环境部署。

2. Spring提供的解决方案
在3.1版本中，spring引入了bean profile功能。通过将可能发生变化的不同的bean定义到一个或多个profile中，当其对应的profile处于激活(active)状态时，则该bean生效。

```

### 初始化**指令处理组件**时，注入**所有指令**，spring在什么时候将**所有指令**加入到容器中的呢？
`原因：因为在spring执行到扫描包的时候，spring将所有带有spring注解的Bean，都注入到spring容器中，所以在初始化指令处理组件时可以将所有指令注入`