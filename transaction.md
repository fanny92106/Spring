# Transaction 


0. 意义
    
        - 保证数据的完整性和一致性
        - 定义: 由一组逻辑上紧密联合而合并成一个整体的多个数据库操作组成, 这些操作要么都执行, 要么都不执行


1. ACID

        - 原子性 (Atomicity) -- 操作层面
            原子性是指事务是一个不可分割的工作单位，事务中的操作要么都发生，要么都不发生
        - 一致性 (Consistency) -- 状态层面
            事务必须使数据库从一个一致性状态变换到另外一个一致性状态，不能有残留的中间状态
        - 隔离性 (Isolation) 
            一个事务的执行不能被其他事务干扰，即一个事务内部的操作及使用的数据对并发的其他事务是隔离的，并发执行的各个事务之间不能相互干扰
        - 持久性 (Durability) -- 结果层面
            一个事务一旦被提交，它对数据库中数据的改变就是永久性的，接下来的其他操作和数据库故障不应该对其有任何影响


2. 3个并发问题

        对于同时运行的多个事务，当这些事务访问数据库中的相同数据时，如果没有采取必要的隔离机制，就会导致各种并发问题: 
        - 脏读: 
            对于两个事务T1 T2, T1读取了已经被 T2 更新但还没有被提交的字段，之后若 T2 回滚，T1 读取的内容就是临时且无效的
        - 不可重复读:
            对于两个事务T1 T2, T1读取了一个字段，然后T2 !!!更新!!! 了该字段且提交，之后 T1 再次读取同一个字段，值就不同了
        - 幻读:
            对于两个事务T1 T2，T1 从一个表中读取了一个字段，然后 T2 在该表中又！！！插入!!!了一些新的行， 之后如果 T1 再次读取同一个表，就会多出几行


3. 4个隔离级别

        - READ UNCOMMITTED (读未提交)：允许事务读取未被其他事务提交的变更
            解决的问题： x
        - READ COMMITED (读已提交数据): 只允许事务读取已经被其他事务提交的变更
            解决的问题: 脏读
        - REPEATABLE READ (可重复读): 确保事务可以多次从一个字段中读取相同的值，在这个事务持续期间，禁止其他事务对这个字段进行更新
            解决的问题: 脏读，不可重复读
        - SERIALIZABLE (串行化/单线程): 确保事务可以从一个表中读取相同的行，在这个事务持续期间，禁止其他事务对该表执行插入，更新和删除操作，所有并发的问题都可以避免，但性能十分低下
            解决的问题: 脏读，不可重复读， 幻读
            
            
4. Spring中操作 Transaction事务

        1). jars
            - spring-tx-5.3.3.jar
            - spring-jdbc-5.3.3.jar
            - spring-orm-5.3.3.jar
            - com.springsource.net.sf.cglib-2.1.3.jar
            - com.springsource.org.aopalliance-1.0.0.jar
            - com.springsource.org.aspectj.weaver-1.6.8.RELEASE.jar
            
        2). Spring配置文件
![springTransactionConfig](imagePool/springTransactionConfig.png)

       3). add @Transactional in service layer
![@TransactionalOnMethod](imagePool/@TransactionalOnMethod.png)


5. @Transactional 注解的属性设置

        1). propagation(事务的传播行为):
            A方法和B方法都有事务, 当A在调用B时, 会将A中的事务传播给B方法, B方法对事务的处理方式就是事务的传播行为 (父传播给子)
            
            - Propagation.REQUIRED: 被调用者使用调用者的事务(default), B用A的事务
            - Propagation.REQUIRES_NEW:  被调用者不使用调用者的事务(挂起), 而使用自己定义的事务处理, 而没有被传播, B用B的事务
            - ..其他
![TransactionParent](imagePool/TransactionParent.png)            
![TransactionChild](imagePool/TransactionChild.png)
            
        2). isolation(隔离级别): 在并发情况下操作数据的一种规定
           - 读未提交/读已提交/可重复读/串行化
           
        3). timeout(超时): 在事务强制回滚前最多可以等待的时间
        4). readOnly(只读): 指定当前事务中的一系列的操作是否为只读, 若设置为只读, 
            mySQL就会在请求访问数据的时候不加锁, 来提高性能; 如果有写操作一定不要设置!
        5). rollbackFor | rollbackForClassName | noRollbackFor | noRollbackForClassName: 设置因什么而回滚, 或不因什么而回滚
