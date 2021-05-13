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
