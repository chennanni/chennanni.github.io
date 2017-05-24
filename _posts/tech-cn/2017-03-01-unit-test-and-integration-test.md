---
layout: post
title:  "工作笔记：用JUnit写Unit Test和Integration Test"
date:   2017-03-01
categories: [tech-cn]
comments: true
---

## 1. 简单的JUnit Test

业务要求是test`SomeValidator.validateXXX(String, List<String>)`这个方法。
这个方法的作用是：pass一个String进去，然后会进行一系列的validate，如果有exception，就写到List<String>里面。

``` java
@Test
public void testValidateXXX_1() {
     String input;
     List<String> exceptionList = new ArrayList<String>();
     input = null ; // input is null, should pass
     SomeValidator.validateXXX(input, exceptionList);
     assertTrue(exceptionList.size()==0);
}
```

## 2. 需要用到Mock的Unit Test

业务要求是，有一个`SomeForm`里面有一个`XXXField`，经过`SomeHelper.populateXXXField()`之后，这个`XXXField`就会被赋值。
而需要测试的也正是`SomeHelper.populateXXXField()`这个方法。
但是在这个方法中，它调用了`SomeDelegate.getXXXFieldInfo()`来从后端取得值然后再放到`XXXField`中。
如果这个delegate的方法有误那么势必会影响到helper中的这个被测试的方法。所以为了分隔dependency，我们可以mock这个delegate的类和方法。

相比常规的JUnit Test，这里多做了一件事，就是新创建一个mock类，然后设置了这个mock类的behavior。

``` java
@RunWith(PowerMockRunner.class)
@PrepareForTest( { SomeHelper.class, SomeDelegate.class })
public class SomeHelperTest {
    @Test
    public void testPopulateXXXField() throws Exception {
        // expect result
        private String XXXField = "FIELD_XXX";

        // prep the output for delegate
        List aToList = new ArrayList();
        aToList.add(...);
        ...

        // prep the delegate
        SomeDelegate aDelegate = mock(SomeDelegate.class);
        // set mocking behavior for delegate
        PowerMockito.whenNew(SomeDelegate.class).withAnyArguments().thenReturn(aDelegate);
        when(aDelegate.getXXXFieldInfo(Mockito.anyList(), Mockito.anyBoolean())).thenReturn(aToList);

        // prep the form
        SomeForm aForm = new SomeForm();

        // ini the helper
        SomeHelper aHelper = new SomeHelper(aForm);
        // call the target method
        aHelper.populateXXXField();
        // check results
        assertEquals(aField, aForm.getXXXField());
    }
}
```

## 3. Integration Test

业务要求是针对新开发的SomeService类做Java和DB的Integration Test，保证这个Service写的读写数据的方法是正确的。
Service类调用了Dao类，Dao类里面使用了Spring JPA连接数据库进行读写。在写Test的时候，可以分为以下几步：

- 获取Service类
- 调用Service类的写方法存储数据
- 调用Service类的读方法读取数据
- 对比写入的数据和读取的数据是否相同

在下面的例子中，我们用`@RunWith(SpringJUnit4ClassRunner.class) `来保证这个JUnit Test在运行的时候Spring会配置它的相关环境。
然后`@ContextConfiguration`告诉Spring去哪里读配置文件。接着，就可以用`@Resource`或者`Autowired`或者`Inject`来取得需要的类，
这个类已经通过Spring DI实例化了。

``` java
@RunWith(SpringJUnit4ClassRunner.class)
@ContextConfiguration(locations = { "classpath:/locaiton_to/xxx-application-context.xml" })
public class SomeServiceIntegrationTest {

    @Resource(name = "someService")
    private SomeService someService;

    @Test
    public void testSomeService() {
        List<StagableRecord> stagableRecordList = buildRecords();
        try {
            someService.stageRecords(stagableRecordList);
            assertEquals("Record A", someService.getRecords().get(0));
            assertEquals("Record B", someService.getRecords().get(1));
            ...
        } catch (Exception e) {
            Assert.fail("Should not have thrown any exception");
        }
    }

    private void buildRecords() {
        ...
    }
}
```

下面看一下配置文件，这个配置文件做了如下几件事：

- 配置context
  - 指明是annotation config
  - 指明去哪里扫描文件
- 声明Transaction Manager
  - 配置是annotation driven
  - 配置对应的Entity Manager Factory
- 声明Entity Manager Factory
  - 配置对应的Data Source
- 声明Data Source
  - 配置连接参数
- 声明Service bean
- 声明Dao bean

``` xml
<beans xmlns="http://www.springframework.org/schema/beans"
       xmlns:context="http://www.springframework.org/schema/context"
       xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
       xmlns:tx="http://www.springframework.org/schema/tx"
       xmlns:util="http://www.springframework.org/schema/util"
       xsi:schemaLocation="http://www.springframework.org/schema/beans http://www.springframework.org/schema/beans/spring-beans-3.1.xsd
                           http://www.springframework.org/schema/tx
                           http://www.springframework.org/schema/tx/spring-tx-3.1.xsd
                           http://www.springframework.org/schema/context
                           http://www.springframework.org/schema/context/spring-context-3.1.xsd
                           http://www.springframework.org/schema/util http://www.springframework.org/schema/util/spring-util-2.0.xsd">
       
      <context:annotation-config/>
      <context:component-scan base-package="com.xxx.xxx.dao,com.xxx.xxx.service"/>
      <tx:annotation-driven transaction-manager="transactionManager"/>

    <bean id="someTxnDataSourceName" class="org.springframework.jdbc.datasource.DriverManagerDataSource" lazy-init="true">
        <property name="driverClassName" value="oracle.jdbc.OracleDriver"/>
        <property name="username" value="username"/>
        <property name="password" value="password"/>
        <property name="url" value="jdbc:oracle:thin:@//xxx.xxx.xxx.com:6666/xxx.xxx.com"/>
    </bean>

    <bean id="entityManagerFactory" class="org.springframework.orm.jpa.LocalContainerEntityManagerFactoryBean" lazy-init="true">
        <property name="persistenceUnitName" value="xxx-common-loader"/>
        <property name="persistenceXmlLocation" value="classpath:/META-INF/persistence.xml"/>
        <property name="loadTimeWeaver">
            <bean class="com.xxx.loader.dao.persistance.LoaderTimeWeaver"/>
        </property>
        <property name="dataSource" ref="someTxnDataSourceName"/>
    </bean>

    <!-- set up transaction manager -->
    <bean id="transactionManager" class="org.springframework.orm.jpa.JpaTransactionManager" lazy-init="true">
        <property name="persistenceUnitName" value="xxx-common-loader" />
        <property name="entityManagerFactory" ref="entityManagerFactory"/>
    </bean>

    <bean id="xxxDao" class="com.xxx.xxx.dao.xxxDaoImpl"/>
    <bean id="xxxService" class="com.xxx.xxx.service.xxxServiceImpl"/>
</beans>
```
