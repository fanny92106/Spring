# Annotation-driven Spring Development

## 容器注入bean


1. @Configuration, @Bean, AnnotationConfigApplicationContext
    
        a. bean
![javaBean](imagePool/annotation-driven/javaBean.png)
    
        b. config class
![configClass](imagePool/annotation-driven/configClass.png)

        c. annotationConfigApplicationContext
![annotationAC](imagePool/annotation-driven/annotationAC.png)


2. @ComponentScan, includeFilters, Filter, FilterType

        a. includeFilters
            - FilterType.ANNOTATION
            - FilterType.ASSIGNABLE_TYPE
            - FilterType.CUSTOM
        b. excludeFilters
    ![componentScanFilter](imagePool/annotation-driven/componentScanFilter.png)

            自定义Filter
    ![customizeFilter](imagePool/annotation-driven/customizeFilter.png)
        

3. @Scope, @Lazy

        a. default scope: singleton
        b. singleton: ioc容器启动时就会调动此方法将单实例创建到ioc容器中
        c. prototype: ioc容器启动时并不会去创建对象放到容器中, 而是在每次获取时才创建对象

![beanScope](imagePool/annotation-driven/beanScope.png)

       配置单例bean懒加载: 只有在被调用时才被加载
![lazyLoadSingletonBean](imagePool/annotation-driven/lazyLoadSingletonBean.png)


4. @Conditional, customize condition
        
        条件性地向IOC容器中引入bean
![conditionalAnnotation](imagePool/annotation-driven/conditionalAnnotation.pnga)
        
        customize Condition:
![customizeCondition](imagePool/annotation-driven/customizeCondition.png)


5. @Import
        
        向容器中快速导入一个通过简单无参数构造器创建的bean
![importAnnotation](imagePool/annotation-driven/importAnnotation.png)
        
       ImportSelector: 导入多个bean (SpringBoot中常用！！！！！！！！！！)
![ImportSelector](imagePool/annotation-driven/ImportSelector.png)
![customizeImportSelector](imagePool/annotation-driven/customizeImportSelector.png)

       ImportBeanDefinitionRegistrar: 根据条件手工注册bean
![importBeanDefRegistrar](imagePool/annotation-driven/importBeanDefRegistrar.png)
![manualRegisterBean](imagePool/annotation-driven/manualRegisterBean.png)


6. FactoryBean<T>

        - 容器中的bean是getObject()返回的对象, 虽然在配置文件注册的是FactoryBean
        - 若想获取FactoryBean对象本身, 添加&符号
        - spring与其他框架整合时常用

        定义一个类实现FactoryBean接口
![FactoryBeanDefine](imagePool/annotation-driven/FactoryBeanDefine.png)
        
       在@Configuration配置文件中注册FactoryBean
![FactoryBeanConfig](imagePool/annotation-driven/FactoryBeanConfig.png)

       获取FactoryBean本身
![getFactoryBean](imagePool/annotation-driven/getFactoryBean.png)


7. @Bean Life Cycle Methods

        method_1: @Bean(initMethod, destroyMethod)
![beanLifeCycleMethod](imagePool/annotation-driven/beanAnnotationLifeCycleMethod.png)

       method_2: 实现InitializingBean, DisposableBean接口
![InitializingBeanDisposableBean](imagePool/annotation-driven/implementInitializingBeanDisposableBean.png)
    
       method_3: 
            @PostContruct: 在bean创建完成并且属性赋值完成来执行初始化方法
            @PreDestroy: 在容器销毁bean之前执行一些清理工作
![postConstructPreDestroy](imagePool/annotation-driven/postConstructPreDestroy.png)

       method_4: BeanPostProcessor
            postProcessBeforeInitialization: 在bean初始化之前的后置工作
            postProcessAfterInitialization: 在bean初始化之后的后置工作
![beanPostProcessor](imagePool/annotation-driven/beanPostProcessor.png)


8. @Value, @PropertySource
    
        - 基本数值
        - SpEL: #{} spring expression
        - ${}: 取出配置文件中的值, 使用@PropertySource在配置文件中引入属性.properties文件

编辑属性文件
![propertiesFile](imagePool/annotation-driven/propertiesFile.png)

将属性文件引入到配置文件
![propertySourceAnnotation](imagePool/annotation-driven/propertySourceAnnotation.png)

赋值属性文件中的值
![valueAnnotation](imagePool/annotation-driven/valueAnnotation.png)


9. @Autowired, @Resource, @Inject
        
        三个注解都支持自动装配:
            - @Autowired: 由Spring定义, 首选
            - @Resource: 由java定义, 默认是按组件名称进行装配;
                         不支持@Primary功能, 以及required=false选项
            - @Inject: 由java定义, 需要导入javax.injext包, 不支持require=false选项
            

10. xxxAware

        - 自定义组件想要注入Spring容器底层的一些组件(ApplicationContext, BeanFactory, xxx);
            自定义组件实现xxxAware接口, 在创建对象的时候, 会调用接口规定的方法注入相关组件
![implementXxxAware](imagePool/annotation-driven/implementXxxAware)


11. @Profile

        - 根据当前的环境动态地切换一系列组件的功能, eg: dev, test, prod
        
        设置bean的不同环境Profile
![profileAnnotation](imagePool/annotation-driven/profileAnnotation.png)
        
      设置环境1: 命令行VM动态参数
![setProfileParameterByVMCmd](imagePool/annotation-driven/setProfileParameterByVMCmd.png)
        
      设置环境2: 代码方式, 分步设置容器环境+创建容器
![setProfileParameterByAnnotationConstructor](imagePool/annotation-driven/setProfileParameterByAnnotationConstructor.png)


12. Summary of Annotation

![annotationSummary](imagePool/annotation-driven/annotationSummary.png)


## AOP

1. basic
            
        - @Aspect 指明切面类
        - @Pointcut, @Before, @After, @AfterReturning, @AfterThrowing
        - @EnableAspectJAutoProxy 启动基于注解的aop模式: aspectJ自动代理
        
        目标类
![AopBasicTargetClass](imagePool/annotation-driven/AopBasicTargetClass.png)
        
        切面类 - 声明切面, 声明切入点表达式
![AopBasicAspectClass](imagePool/annotation-driven/AopBasicAspectClass.png)        
        
        配置类 - 开启基于注解的aop模式
![AopBasicConfigClass](imagePool/annotation-driven/AopBasicConfigClass.png)   
        
        运行测试类
![AopBasicTestClass](imagePool/annotation-driven/AopBasicTestClass.png)   


2. @EnableAspectJAutoProxy 的原理

        a. 向容器中添加AnnotationAwareAspectJAutoProxyCreator组件
        b. AnnotationAwareAspectJAutoProxyCreator组件对bean进行postProcessAfterInitialization操作
![AnnotationAwareAspectJAutoProxyCreator](imagePool/annotation-driven/AnnotationAwareAspectJAutoProxyCreator.png)
        
        c. 代理对象(proxy)的执行目标方法 -- 拦截器的链式调用:
![proxyChainCall_1](imagePool/annotation-driven/proxyChainCall_1.png)
![proxyChainCall_2](imagePool/annotation-driven/proxyChainCall_2.png)


## BeanFactoryPostProcessor -- Interface

![ImplementBeanDefinitionRegistryPostProcessor](imagePool/annotation-driven/ImplementBeanDefinitionRegistryPostProcessor.png)

1. 定义
    
        - beanFactory的后置处理器: 在BeanFactory标准初始化之后调用, 来定制和修改BeanFactory的内容, 
            所有的bean定义已经保存加载到beanFactory, 但是bean的实例还未创建
            
2. 原理

        - 在实例化容器时, 调用invokeBeanFactoryPostProcessors(beanFactory), 来注册所有的beanFactoryPostProcessors
        - 在初始化创建其他组件之前执行
        
        
## BeanDefinitionRegistryPostProcessor -- BeanFactoryPostProcessor的子接口

![BeanDefinitionRegistryPostProcessorExtends](imagePool/annotation-driven/BeanDefinitionRegistryPostProcessorExtendsBeanFactoryPostProcessor.png)
![ImplementBeanFactoryPostProcessor](imagePool/annotation-driven/ImplementBeanFactoryPostProcessor.png)

1. 原理

        - postProcessBeanDefinitionRegistry()在所有bean定义信息将要被加载之前, bean实例还未创建时加载
        - 优先于BeanFactoryPostProcessor执行, 利用BeanDefinitionRegistryPostProcessor给容器中再额外添加一些组件
        
2. 执行顺序:

        - 先执行postProcessBeanDefinitionRegistry()方法
        - 再执行postProcessBeanFactory()方法


## ApplicationListener<ApplicationEvent>

1. event-driven development:

        a.监听一个事件:
            1).实现ApplicationListener<ApplicationEvent>接口
![ImplementApplicationListener](imagePool/annotation-driven/ImplementApplicationListener.png)

                - 写一个监听器来监听某个事件(ApplicationEvent及其子事件)
                - 把监听器加入到容器中
                - 只要容器中有相关的时间发布, 就可以监听到这个事件, 调用这个监听器的回调 onApplicationEvent()
                
            2).@EventListener 注解在普通组件上
![EventListenerAnnotation](imagePool/annotation-driven/EventListenerAnnotation.png)

        b. 手动发布一个事件:
![publishEvent](imagePool/annotation-driven/publishEvent.png)
        

2. 事件监听原理: 

![EventListening](imagePool/annotation-driven/EventListening.png)


## IOC容器创建过程

AbstractApplicationContext的 refresh(...):
![refresh](imagePool/annotation-driven/refresh.png)

        1. beanFactory标准初始化
            a. prepareRefresh 创建beanFactory前的预处理
                - 记录启动时间, 设置容器的active和close状态
                - initPropertySource() 初始化属性, 覆写子容器自定义属性设置方法
                - getEnvironment().validateRequiredProperties() 校验环境属性值的合法性
                - earlyApplicationListeners/ applicationListeners 设置早期事件监听器
                - earlyApplicationEvents 设置早期事件, 待事件多播器创建后执行
                
            b. obtainFreshBeanFactory 获取新鲜的beanFactory实例
                - refreshBeanFactory() 创建beanFactory实例
                    a. 创建一个DefaultListableBeanFactory实例
                    b. beanFactory.setSerializationId() 给工厂设置序列化id
                - getBeanFactory() 返回所创建的beanFactory实例
            
            c. prepareBeanFactory(beanFactor) 创建beanFactory之后的准备工作
                - 设置beanFactory的类加载器, 表达式解析器等等..
                - 添加beanPostProcessors后置处理器
                - 设置忽略的自动装配接口
                - 注册可解析的组件: beanFactory, resourceLoader, applicationEventPublisher, applicationContext
                - 添加编译时的AspectJ xxxWeaver
                - 注册环境/系统变量组件
            
            d. postProcessBeanFactory(beanFactory) beanFactory的后置处理工作
                - 默认为空, 自定义子类可重写这个方法在beanFactory做进一步的设置
                
        2. beanFactory中添加beanFactoryPostProcessors, beanPostProcessors, eventListener, multicaster, 以及创建其他单实例bean
            a. invokeBeanFactoryPostProcessors 调用beanFactory的后置处理器
                - BeanFactoryPostProcessor在整个BeanFactory标准初始化完成后进行拦截调用
                - BeanDefinitionRegistryPostProcessor继承了BeanFactoryPostProcessor, 在beanFactory解析完所有的BeanDefinition后拦截调用
                - BeanFactoryPostProcessor来源:
                    * 通过ApplicationContext的addBeanFactoryPostProcessor()方法手动添加自己的拦截器
                    * 系统默认了一些BeanFactoryPostProcessor, 例如: ConfigurationClassPostProcessor用来处理@Configuration标注的Spring配置类
                - 调用顺序:
                    1). 先调用BeanDefinitionRegistryPostProcessor类型的实现了PriorityOrderd, Ordered接口的postProcessors, 然后调用普通的BeanDefinitionRegistryPostProcessor
                    2). 先调用BeanFactoryPostProcessor类型的实现了PriorityOrderd, Ordered接口的postProcessors, 然后调用普通的BeanFactoryPostProcessor
        
            b. registerBeanPostProcessors 注册bean的后置处理器
                - 从beanFactory里获取所有BeanPostProcessor类型的Bean的名称
                - 调用beanFactory的getBean方法, 并传入每一个BeanPostProcessor类型的Bean名称, 从容器中获取该Bean的实例
                - 执行顺序:
                    1). 向beanFacotory注册实现了PriorityOrdered的BeanPostProcessor类型的Bean实例
                    2). 向beanFactory注册实现了Ordered的BeanPostProcessor类型的Bean实例
                    3). 向beanFactory注册普通的BeanPostProcessor类型的Bean实例
                    4). 向beanFactory重新注册实现了MergedBeanDefinitioPostProcessor的BeanPostProcessor类型的Bean实例
                - 向beanFactory注册BeanPostProcessor的过程就是简单地将实例保存到beanFactory的beanPostProcessor属性中
                
            c. 
            
            
