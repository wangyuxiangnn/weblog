---
title: springboot相关配置 
categories: 框架知识  
date: 2018-04-11 20:35:16
preview: https://images.pexels.com/photos/823841/pexels-photo-823841.jpeg?auto=compress&cs=tinysrgb&h=750&w=1260
subtitle: 不知道该写什么，多百度，总会有发现
---
1、SpringBoot注解作用:
######    @SpringBootApplication这个Spring Boot核心注解是由其它三个重要的注解组合，分别是： 

    @SpringBootConfiguration:
      是SpringBoot项目的配置注解，这也是一个组合注解包含 @Configuration
   
    @EnableAutoConfiguration:
      注解用于启用自动配置，该注解会使Spring Boot根据项目中依赖的jar包自动配置项目的配置项。
      
    @ComponentScan:
      是组件扫描注解，不配置默认扫描@SpringBootApplication所在类的同级目录以及它的子目录(这很重要,后面很应用到这个特性)。当然你也可以自己指定要扫描的 包目录
      例如: @ComponentScan(basePackages = "com.lqr.dem")

2、取消部分自动配置:

     @SpringBootApplication(exclude={DataSourceAutoConfiguration.class,HibernateJpaAutoConfiguration.class})
     
3、全局配置文件（application.yml）:

     yml文件在书写时，需要注意一个地方：冒号与值中间是存在空格的！
     1.自定义属性
     yml配置文件:
         wis: hive
         content: "${wis} ok"
         
     java代码:
         @Value("${wis}")
         


---
4、Web编码:
   
  
```
1. RestController
    @RestController = @Controller + @ResponseBody
2. http组合注解
@GetMapping => 
	@RequestMapping(value = "/xxx",method = RequestMethod.GET)
@PostMapping => 
	@RequestMapping(value = "/xxx",method = RequestMethod.POST)
@PutMapping => 
	@RequestMapping(value = "/xxx",method = RequestMethod.PUT)
@DeleteMapping => 
	@RequestMapping(value = "/xxx",method = RequestMethod.DELETE)

3. 数据校验
    1）@Valid + BindingResult
    例:
    /**
     * 添加一个女生
     */
    @PostMapping(value = "/girls")
    public Result<Girl> girlAdd(@Valid Girl girl, BindingResult bindingResult) {
        if (bindingResult.hasErrors()) {
            return ResultUtils.error(-1, bindingResult.getFieldError().getDefaultMessage());
        }
        return ResultUtils.success(girlRespository.save(girl));
    }
    2）设置校验规则
    例:
    public class Girl {
        
        private Integer id;
    
        @NotBlank(message = "这个字段必传")
        private String cupSize;
    
        @Min(value = 18, message = "未成年少女禁止入内")
        private Integer age;
    
        @NotNull(message = "金额必传")
        private Double money;

    }

```

   
     