# Annotation-driven Spring Development


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

