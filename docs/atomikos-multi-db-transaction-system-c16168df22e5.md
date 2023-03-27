# atomikos——多数据库事务系统

> 原文：<https://medium.com/swlh/atomikos-multi-db-transaction-system-c16168df22e5>

![](img/a0c7ca46e8a8a492252b8771e89d7db4.png)

最终一致性、弹性、微服务、CQRS、十二因素应用程序或反应性不再是热门词汇。大多数开发者声称[反应宣言](https://www.reactivemanifesto.org)， [12 因素应用](https://12factor.net)是构建反应灵敏的网络应用的最好也是唯一的方法。*事务*主要存在于服务或数据库中，术语*传播事务*听起来很可疑。我完全同意这一点，但是当你处理一个带有多个数据库的老式整体系统时，你很可能会与这个地狱作斗争。

在这种情况下， [Atomikos](https://www.atomikos.com) 听起来是最好的解决方案。根据[网页](https://www.atomikos.com)介绍，Atomikos 是一个面向 Java 和 REST 的云原生事务管理系统。但这意味着什么呢？Atomikos 是一个支持多数据库事务的库，包括消息和 REST。更重要的是，不需要交易服务器，根据他们的网页，这是唯一支持交易服务器的库。

## 用例

我甚至不会考虑 Atomikos，但是有一天，我正在使用一个中型的分布式 monolith 为企业客户提供 SaaS 解决方案。其中一个关键问题是在客户端之间分离数据。虽然对他们中的大多数人来说，隐私是最重要的，但我们决定选择物理隔离，而不是逻辑隔离。使用 Spring，在数据源之间路由非常容易。查看[这个故事](/@szczerbicki.pawel/transparent-datasource-routing-in-spring-9fbfb2a9664)了解更多信息。但是，如果您必须存储进入您系统的每个新客户机，然后引导一个单独的数据库，那该怎么办呢？作为一个团队，我们决定开发一个管理系统，该系统预先准备好虚拟环境，然后在成功注册后分配它。这个流需要在两个数据库中插入多个条目:client 和 admin，并在一个原子事务中发送一些事件。为什么？别问了——这是许多错误决定的结果。但是，对于这种情况，Atomikos 听起来是完美的解决方案——事实也的确如此。

## 解决办法

Spring 处理开箱即用的单一数据源。对于多个 DS，需要额外的配置，尤其是当您需要分散事务时。

首先，你必须从`javax.sql.Datasource`切换到`com.atomikos.jdbc.nonxa.AtomikosNonXADataSourceBean`，这当然实现了`javax.sql.Datasource`，并且内置了一个连接池。要创建`Datasource`，只需设置新的 bean

```
AtomikosNonXADataSourceBean ds = new AtomikosNonXADataSourceBean();
ds.setDriverClassName(*DRIVER*);
ds.setUrl(endpoint);
ds.setUser(user);
ds.setPassword(password);
ds.setUniqueResourceName(*randomAlphabetic*(10));
ds.setTestQuery(*SQL*);
ds.setMaxPoolSize(*MAX_CONNECTIONS_PER_DATABASE*);
```

请注意，使用`AtomikosNonXADataSourceBean`对于恢复是不安全的。为此，请使用`XA`数据源。

然后用 Atomikos 提供的事务管理器覆盖默认的 Spring 事务管理器。使用`UserTransactionManager` ，根据[文档](https://www.atomikos.com/downloads/transactions-essentials/com/atomikos/AtomikosTransactionsEssentials/javadoc/3.7/com/atomikos/icatch/jta/UserTransactionManager.html)它是`TransactionManager`的零设置实现

```
@Bean(destroyMethod = "close", initMethod = "init")
public TransactionManager atomikosTransactionManager() {
    UserTransactionManager userTransactionManager = new                    UserTransactionManager();
    userTransactionManager.setForceShutdown(false);
    userTransactionManager.setTransactionTimeout(*TRANSACTION_TIMEOUT*);
    return userTransactionManager;
}
```

创建允许应用程序显式管理事务的`UserTransaction`对象。只有当用户希望创建自己的事务管理器时，才应该使用这个类，我们希望使用 Atomikos 来实现这一点

```
@Bean
public UserTransaction userTransaction() throws SystemException {
    UserTransactionImp userTransaction = new UserTransactionImp();
    userTransaction.setTransactionTimeout(*TRANSACTION_TIMEOUT*);
    return userTransaction;
}
```

然后最终覆盖`PlatformTransactionManager`，根据 [Spring 文档](https://docs.spring.io/spring/docs/current/javadoc-api/org/springframework/transaction/PlatformTransactionManager.html)它是 Spring 事务基础设施中的中央接口。Atomikos 建议使用`JtaTransactionManager`,它必须与我们之前创建的`UserTransaction`和`TransactionManager`一起使用

```
@Bean(name = "transactionManager")
public PlatformTransactionManager transactionManager(UserTransaction userTransaction, TransactionManager atomikosTransactionManager) throws Throwable {
    JtaTransactionManager jtaTransactionManager = new JtaTransactionManager(userTransaction, atomikosTransactionManager);
    jtaTransactionManager.setDefaultTimeout(*TRANSACTION_TIMEOUT*);
    return jtaTransactionManager;
}
```

现在只需使用`@EnableTransactionManagement`启用交易，就大功告成了！

## 摘要

即使我们生活在一个事件驱动和最终一致性微服务架构的时代，多系统事务管理系统仍然至关重要。性感的词，如反应，排队等。并不适用于我们所有的设计。架构师在他们的方法上走得太远了，试图用微服务解决所有的问题，这通常会导致一个充满设计陷阱和错误的分布式整体。在某些情况下，特别是当软件还年轻的时候，模块化的整体正是你所需要的。而且有时候应该停留在这个水平。然后，您可能需要 multi db 的事务系统，Atomikos 将很好地为您工作。对于一个分布式的整体，它在我的情况下工作得很好。我不鼓励您工作或编写这样的代码，但是当您不知何故陷入这个陷阱时，请将 Atomikos 视为一个事务管理系统。