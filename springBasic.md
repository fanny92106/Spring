# Spring_1


Table of Contents

    1.Background
    2.Spring特性
    3.Sping模块
    4.IOC & DI
    5.Spring 从容器中获取bean的方式
    6.ApplicationContext 的主要实现类
    7.依赖注入的方式
    8.可以注入的值
    
    


1. Background

    before: servlet + jsp
    struts1 encapsulate s ervlet (20 - 30 years ago) -> reduce code  [deprecated]
    struts2 encapsulate filter (10 years ago)                               [deprecated]


    MVC framework:

        - struts1 (apache)
        - struts2 (apache)
        - springMVC
        
    Persistent framework:
        
        - Hibernate (jboss)
        - Mybatis (apache)

    Integrated/design framework: 

        - spring (以后什么都不用但spring一定会用)



2. Spring 特性 

    - 核心: IoC 和 AOP
    - 特性：
            
            a. 非侵入性 (轻量级) : 
                基于spring开发的应用不依赖于spring的api， 对原来的技术不造成影响

            b. DI:
                反转控制(IOC)最经典的实现
                IOC  是一种思想，DI 是 IOC的一种实现
            
            c. AOP: 对OOP进行补充
            
            d. Spring 是一个容器，因为它包含并且管理应用对象的声明周期
            
            e. 组件化: 实现了使用简单的组件配置组合成一个复杂的应用。组件就是spring管理的对象。以前不是的，解耦了
            
            f. 一站式: 可以整合各种企业应用的开源框架，优秀的第三方类库



3. Spring 模块

![springModules](imagePool/springModules.png)

    - 最底层最核心: IOC  （beans/ core/ context)



4. IOC (Inversion of Control) & DI (Dependency Injection)

IOC: 
    
    - 传统开发方式:
        组件主动地从容器中获取所需要的资源，因此开发人员需要知道在具体容器中特定资源的获取方式，增加了学习成本，同时降低了开发效率
    - 控制反转: 
        改变了资源的获取方向 -- 改由容器主动地将资源推送给需要的组件，开发人员buxuyao9知道容器是如何创建资源对象的，只需要提供接收资源的方式即可，极大地降低了学习成本，提高了开发效率
    
    - 大白话版: 把程序员er对一个对象的管理权反转给程序本身 (尚硅谷)
    
DI:  (Injection = 赋值)

    - 把有依赖关系的属性进行赋值，eg: setter()
    
    - 大白话版: 给bean的属性赋值 (尚硅谷)
    
    
    
5. Spring 从容器中获取bean的方式

    1) use bean id

        Object student = (Student) contxt.getBean("bean_id");
        
    2) use class, but must have only one bean configured for this class
    
        Student student = context.getBean( Student.class );
        
    3) use bean id and class -- recommend !!!!
    
        Student student = context.getBean("student1", Student.class);
        


6. ApplicationContext 的主要实现类

    - ClassPathXmlApplicationContext: 对应classpath 下的xml格式的配置文件，使用的是相对路径
    - FileSystemXmlApplicationContext: 对应文件系统中的xml格式的配置文件，使用的是绝对路径
    
    

7. 依赖注入的方式

### setter injection
    
![setterInjection](imagePool/setterInjection.png)
    
### constructor injection (rarely use)

![constructorInjection](imagePool/constructorInjection.png)




8. 可以注入的值

### 字面量 -- 使用 value 来赋值

        - 可以使用字符串表示的值，可以通过value 属性或 value 子节点的方式指定
![valueSubNode](imagePool/valueSubNode.png)

        - 8种基本数据类型及其封装类，String等类型都可以用采用字面值注入的方式
    
    
### 非字面量/类型 -- 使用 ref 来赋值

        - ref 代表 配置文件中spring 管理某个 bean id
![refInjection](imagePool/refInjection.png)

        - 定义内部bean ref, 只能供当前bean 使用，不能被外部其他bean引用
![refInternalBeanInjection](imagePool/refInternalBeanInjection.png)        



### 集合的赋值
    
        - List<String>        
![listInjection](imagePool/listInjection.png)        

        - List<Student>
![listClassTypeInjection](imagePool/listClassTypeInjection.png)
        
        - Map<String, String>
![mapInjection](imagePool/mapInjection.png)



