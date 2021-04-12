# AOP

    AOP底层原理 = 动态代理 + try-catch-finally block
    
0. AOP -- 面向切面编程

        - 面向对象  纵向继承机制
        - 面向切面  横向抽取机制


1. AOP 术语
    
               (站在目标对象角度) 横切关注点 == 通知 (站在切面角度)

        - 横切关注点  从每个方法中抽取出来的同一类非核心业务, eg: logging, 站在目标对象的角度
        - 切面(aspect)  封装横切关注点信息的类, 每个关注点体现为一个通知方法
        - 通知(advice)  切面必须完成的具体工作, 即是横切关注点, 只不过是站在切面的角度来讲; 一共有5种
        - 目标对象(target)  所抽取出来的代码要作用到的对象
        - 代理(proxy)  向目标对象应用通知之后创建的代理对象
        - 连接点(joint point)  横切关注点在程序代码中执行的特定位置, eg: 前 后 返回 错误
        - 切入点(point cut)  表达式, 条件


2. Jars 
        
        - spring-aop-5.3.3.jar
        - spring-aspects-5.3.3.jar
        ******* aspectj jars ******
        - com.springsource.org.aspectj.weaver-1.6.8.RELEASE.jar
        - com.springsource.org.aopalliance-1.0.0.jar
        - com.springsource.net.sf.cglib-2.1.3.jar
