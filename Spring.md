# Spring

## IoC操作Bean管理

IoC（Inversion of ConTrol ，控制反转)

Bean管理就是通过Spring创建对象、注入属性

### 基于xml

#### set注入

Student类

```java
package com.x17.pojo;
public class Student {
    private String name;
    private int age;
    构造器、getter、setter、toString略
}
```

bean.xml

```xml
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
       xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
       xsi:schemaLocation="http://www.springframework.org/schema/beans http://www.springframework.org/schema/beans/spring-beans.xsd">
    <!--创建对象-->
    <bean id="student" class="com.x17.pojo.Student">
        <!--注入属性-->
        <property name="name" value="17"/>
        <property name="age" value="20"/>
    </bean>
</beans>
```
StudentTest

```java
@Test
public void StudentTest(){
//  启动Spring容器
   ApplicationContext context=new ClassPathXmlApplicationContext("bean.xml");
   Student student=context.getBean("student",Student.class);//获取实例化对象
   System.out.println(student);
   //Student{name='17', age=20}
}
```

> **注意：该方法创建对象先执行空参构造器，所以必须要有空参构造器。且属性注入是通过setter注入，必须要有setter方法**

#### 构造注入

student类和测试同上

bean.xml

```xml
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
       xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
       xsi:schemaLocation="http://www.springframework.org/schema/beans http://www.springframework.org/schema/beans/spring-beans.xsd">
    <!--   方式一，通过index，0就是第一个参数，以此类推-->
    <!--    <bean id="student" class="com.x17.pojo.Student">-->
    <!--        <constructor-arg index="0" value="17"/>-->
    <!--        <constructor-arg index="1" value="20"/>-->
    <!--    </bean>-->
    <!--    方式二，省略index，根据先后顺序填充构造器参数-->
    <!--    <bean id="student" class="com.x17.pojo.Student">-->
    <!--        <constructor-arg value="17"/>-->
    <!--        <constructor-arg value="20"/>-->
    <!--    </bean>-->
    <!--  方式三，通过name，（常用）  -->
    <bean id="student" class="com.x17.pojo.Student">
        <constructor-arg name="name" value="17"/>
        <constructor-arg name="age" value="20"/>
    </bean>
</beans>
```

> **此方式必须有带参构造器**

#### P命名空间注入

student类和测试同上

bean.xml

```xml
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
       xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
        <!--添加P命名空间-->
       xmlns:p="http://www.springframework.org/schema/p"
       xsi:schemaLocation="http://www.springframework.org/schema/beans http://www.springframework.org/schema/beans/spring-beans.xsd">
    <bean id="student" class="com.x17.pojo.Student"
    p:name="17" p:age="20"> <!--注入属性-->
    </bean>
</beans>
```

> **注意：该方法创建对象先执行空参构造器，所以必须要有空参构造器。且属性注入是通过setter注入，必须要有setter方法**

#### 注入特殊值

1、使用转义字符

2、使用CDATA

```xml
<value><![CDATA[特殊值]]></value>
```

student类和测试同上

bean.xml

```xml
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
       xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
       xsi:schemaLocation="http://www.springframework.org/schema/beans http://www.springframework.org/schema/beans/spring-beans.xsd">
    <bean id="student" class="com.x17.pojo.Student">
        <property name="name">
            <value><![CDATA[<<17>>]]></value>
        </property>
        <property name="age" value="20"/>
    </bean>
</beans>
```

测试输出：Student{name='<<17>>', age=20}

#### 注入外部Bean

Student类

```java
package com.x17.pojo;
public class Student {
    private String name;
    private int age;
    private Teacher teacher;
    构造器、getter、setter、toString略
}
```

Teacher类

```java
package com.x17.pojo;

public class Teacher {
    private String name;
    private int age;
    构造器、getter、setter、toString略
}
```

bean.xml

```xml
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
       xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
       xsi:schemaLocation="http://www.springframework.org/schema/beans http://www.springframework.org/schema/beans/spring-beans.xsd">
    <bean id="student" class="com.x17.pojo.Student">
        <property name="name" value="17"/>
        <property name="age" value="20"/>
        <property name="teacher" ref="teacher"></property>
    </bean>
    <bean id="teacher" class="com.x17.pojo.Teacher">
        <property name="name" value="王老师"/>
        <property name="age" value="42"/>
    </bean>
</beans>
```

测试不变，结果为：Student{name='17', age=20, teacher=Teacher{name='王老师', age=42}}

#### 注入内部Bean

Student类、Teacher类、测试同上

bean.xml

```xml
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
       xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
       xsi:schemaLocation="http://www.springframework.org/schema/beans http://www.springframework.org/schema/beans/spring-beans.xsd">
    <bean id="student" class="com.x17.pojo.Student">
        <property name="name" value="17"/>
        <property name="age" value="20"/>
        <property name="teacher">
            <bean id="teacher" class="com.x17.pojo.Teacher">
                <property name="name" value="王老师"/>
                <property name="age" value="42"/>
            </bean>
        </property>
    </bean>
</beans>
```

测试结果为：Student{name='17', age=20, teacher=Teacher{name='王老师', age=42}}

#### 注入集合属性

Teacher类、测试同上

Student类

```java
package com.x17.pojo;
import java.util.*;
public class Student {
    private String name;
    private int age;
    private Teacher[] teacher;
    private List course;
    private Map<String,Integer> score;
    private Set<String> club;
    private Properties info;
    构造器、getter、setter、toString略
}
```

bean.xml

```xml
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
       xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
       xsi:schemaLocation="http://www.springframework.org/schema/beans http://www.springframework.org/schema/beans/spring-beans.xsd">
    <bean id="student" class="com.x17.pojo.Student">
        <property name="name" value="17"/>
        <property name="age" value="20"/>
        <property name="teacher">
            <array>
                <ref bean="teacher1"/>
                <ref bean="teacher2"/>
            </array>
        </property>
        <property name="course">
            <list>
                <value>语文</value>
                <value>数学</value>
                <value>英语</value>
            </list>
        </property>
        <property name="score">
            <map>
                <entry key="语文" value="96"/>
                <entry key="数学" value="96"/>
                <entry key="英语" value="96"/>
            </map>
        </property>
        <property name="club">
            <set>
                <value>计算机俱乐部</value>
                <value>机器人协会</value>
            </set>
        </property>
        <property name="info">
            <props>
                <prop key="1">哈哈哈</prop>
                <prop key="2">呜呜呜</prop>
            </props>
        </property>
    </bean>
    <bean id="teacher1" class="com.x17.pojo.Teacher">
        <property name="name" value="王老师"/>
        <property name="age" value="42"/>
    </bean>
    <bean id="teacher2" class="com.x17.pojo.Teacher">
        <property name="name" value="李老师"/>
        <property name="age" value="43"/>
    </bean>
</beans>
```

测试结果：Student{name='17', age=20, teacher=[Teacher{name='王老师', age=42}, Teacher{name='李老师', age=43}], course=[语文, 数学, 英语], score={语文=96, 数学=96, 英语=96}, club=[计算机俱乐部, 机器人协会], info={2=呜呜呜, 1=哈哈哈}}

#### bean的实例化管理

在 Spring 里面，默认情况下，bean 是单例模式，即创建单实例对象。

测试（将Teacher类的toString()去掉。bean.xml中配置一个Teacher Bean

```java
@Test
public void TeacherTest(){
    ApplicationContext context=new ClassPathXmlApplicationContext("bean.xml");
    Teacher teacher1=context.getBean("teacher",Teacher.class);
    Teacher teacher2=context.getBean("teacher",Teacher.class);
    System.out.println(teacher1);//com.x17.pojo.Teacher@5d11346a
    System.out.println(teacher2);//com.x17.pojo.Teacher@5d11346a
 }
```

地址相同，确实是单实例对象

如果不希望他是单实例对象，可以修改Bean配置的scope属性。

- singleton，表示是单实例对象 （默认值）
- prototype，表示是多实例对象

```xml
<bean id="teacher" class="com.x17.pojo.Teacher" scope="prototype">
    <property name="name" value="李老师"/>
    <property name="age" value="43"/>
</bean>
```

再次测试结果为

com.x17.pojo.Teacher@7a36aefa
com.x17.pojo.Teacher@17211155

> **提示：不要改变默认的单例配置。Spring的核心功能就是对Bean进行合理管理，在实际开发中很少见到取消单例配置的处理操作**

#### bean 生命周期

1. 通过构造器创建 bean 实例（无参构造）

2. 为 bean 的属性设置值和对其他 bean 引用（调用 set 方法）

3. 把 bean 实例传递 bean 后置处理器的方法 postProcessBeforeInitialization

4. 调用 bean 的初始化的方法（需要进行配置初始化的方法）

5. 把 bean 实例传递 bean 后置处理器的方法 postProcessAfterInitialization

6. bean 可以使用了（对象获取到了）

7. 当容器关闭时候，调用 bean 的销毁的方法（需要进行配置销毁的方法）

Kid类

```java
package com.x17.beanLive;
public class Kid {
    private String name;
    public Kid() {
        System.out.println("创建Bean实例：怀孕了");
    }
    public void setName(String name) {
        this.name = name;
        System.out.println("为 bean 的属性设置值：取名为" + name);
    }
    public void init() {
        System.out.println("初始化：快生了");
    }
    public void destory() {
        System.out.println("孩子挂了");
    }
}
```

POST类

```java
package com.x17.beanLive;
import org.springframework.beans.BeansException;
import org.springframework.beans.factory.config.BeanPostProcessor;
public class Post implements BeanPostProcessor {
    //后置处理器：初始化之前
    public Object postProcessBeforeInitialization(Object bean, String beanName) throws BeansException {
        System.out.println("初始化之前执行：身体检查");
        return bean;
    }
    //后置处理器：初始化之后
    public Object postProcessAfterInitialization(Object bean, String beanName) throws BeansException {
        System.out.println("初始化之后执行：接生");
        return bean;
    }
}
```

bean.xml

```xml
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
       xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
       xsi:schemaLocation="http://www.springframework.org/schema/beans http://www.springframework.org/schema/beans/spring-beans.xsd">
    <bean id="kid" class="com.x17.beanLive.Kid"
          <!--这里说明初始方法和销毁方法-->
          init-method="init" destroy-method="destory">
        <property name="name" value="二狗"/>
    </bean>
    <bean id="post" class="com.x17.beanLive.Post"/>
</beans>
```

测试

```java
@Test
public void kidTest(){
    ClassPathXmlApplicationContext context=new ClassPathXmlApplicationContext("bean.xml");
    Kid kid=context.getBean("kid", Kid.class);
    System.out.println("出生了");
   context.close();
}
```

结果：

- 创建Bean实例：怀孕了
- 为 bean 的属性设置值：取名为二狗
- 初始化之前执行：身体检查
- 初始化：快生了
- 初始化之后执行：接生
- 出生了
- 孩子挂了

> **注意：加了后置处理器后，每个Bean实例都会使用**

#### 自动装配

autowire属性：

byType:通过类型自动装配

byName：通过名称自动装配

```xml
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
       xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
       xsi:schemaLocation="http://www.springframework.org/schema/beans http://www.springframework.org/schema/beans/spring-beans.xsd">
    <bean id="student" class="com.x17.pojo.Student" autowire="byName">
        <property name="name" value="小明"/>
        <property name="age" value="19"/>
    </bean>
    <bean id="teacher" class="com.x17.pojo.Teacher"/>
</beans>
```

#### 注入外部属性

jdbc.properties

```properties
prop.driverClass=com.mysql.jdbc.Driver
prop.url=jdbc:mysql://localhost:3306/BookDb
prop.userName=root
prop.password=123456
```

bean.xml

```xml
<beans xmlns="http://www.springframework.org/schema/beans"
       xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
       xmlns:context="http://www.springframework.org/schema/context"
       xsi:schemaLocation="http://www.springframework.org/schema/beans http://www.springframework.org/schema/beans/spring-beans.xsd
                           http://www.springframework.org/schema/context http://www.springframework.org/schema/context/spring-context.xsd"><!--引入context名称空间-->
    
        <!--引入外部属性文件-->
    <context:property-placeholder location="classpath:jdbc.properties"/>

    <!--配置连接池-->
    <bean id="dataSource" class="com.alibaba.druid.pool.DruidDataSource">
        <property name="driverClassName" value="${prop.driverClass}"></property>
        <property name="url" value="${prop.url}"></property>
        <property name="username" value="${prop.userName}"></property>
        <property name="password" value="${prop.password}"></property>
    </bean>
</beans>
```

### 基于注解

**打开注解**

bean.xml

```xml
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
       xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
       xmlns:context="http://www.springframework.org/schema/context"
       xsi:schemaLocation="http://www.springframework.org/schema/beans http://www.springframework.org/schema/beans/spring-beans.xsd
                           http://www.springframework.org/schema/context http://www.springframework.org/schema/context/spring-context.xsd">
    <!--开启注解扫描，base-package指明要扫描的包-->
    <!--不同的包用逗号隔开，也可以直接使用上层包-->
    <context:component-scan base-package="com.x17"></context:component-scan>
<!-- use-default-filters="false" 表示现在不使用默认 filter，自己配置 filter-->
<!-- context:include-filter ，设置扫描哪些内容-->
<!--    <context:component-scan base-package="com.x17" use-default-filters="false">-->
<!--        <context:include-filter type="annotation"-->
<!--                                expression="org.springframework.stereotype.Controller"/>&lt;!&ndash;代表只扫描Controller注解的类&ndash;&gt;-->
<!--    </context:component-scan>-->
<!--     下面配置扫描包所有内容-->
<!--     context:exclude-filter： 设置哪些内容不进行扫描-->
<!--    <context:component-scan base-package="com.x17">-->
<!--        <context:exclude-filter type="annotation"-->

<!--                                expression="org.springframework.stereotype.Controller"/>&lt;!&ndash;表示Controller注解的类之外一切都进行扫描&ndash;&gt;-->
<!--    </context:component-scan>-->

</beans>
```

#### 创建对象

- @Component：定义组件
- @Service：业务层注解
- @Controller：控制层注解
- @Repository：数据层注解

> @Service、@Controller、@Repository3个注解，打开源代码可以发现都包含@Component注解。实际上这4个注解功能是完全相同的，Spring为了描述的准确性，才设计了不同的名称

Student类

```java
package com.x17.pojo;

import org.springframework.stereotype.Component;
//value可以省略不写，默认就是类名首字母小写
@Component(value = "student")
public class Student {
    private String name;
    private int age;
    private Teacher teacher;
}    
```

#### 注入属性

@Autowired：根据属性类型进行自动装配

@Qualifier：根据名称进行注入，和@Autowired 一起使用

@Resource：可以根据类型注入，也可以根据名称注入（它属于javax包下的注解，不推荐使用！）

@Value：注入普通类型属性

Student类

```java
package com.x17.pojo;
import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.beans.factory.annotation.Value;
import org.springframework.stereotype.Component;
@Component
public class Student {
    @Value(value = "17")
    private String name;
    @Value(value = "20")
    private int age;
    @Autowired
    private Teacher teacher;
    //不需要setter方法，toString略
}
```

Teacher类

```java
package com.x17.pojo;
import org.springframework.beans.factory.annotation.Value;
import org.springframework.stereotype.Component;
@Component
public class Teacher {
    @Value(value = "王老师")
    private String name;
    @Value(value = "43")
    private int age;
    //不需要setter方法，toString略
}
```

#### 完全注解开发

创建配置类，替代 xml 配置文件

```java
package com.x17.config;
import org.springframework.context.annotation.ComponentScan;
import org.springframework.context.annotation.Configuration;
@Configuration //作为配置类，替代 xml 配置文件
@ComponentScan(basePackages = {"com.x17"})
public class SpringConfig {
}
```

## AOP

AOP是一种编码范式，是**基于动态代理**的一种更高级的应用，可**结合AspectJ组件**，利用切面表达式织入到程序组成中，实现组件的解耦合设计。

- 连接点：类里面哪些方法可以被增强，这些方法称为连接点

- 切入点：实际被真正增强的方法称为切入点

- 通知（增强）：实际增强的逻辑部分称为通知，且分为以下五种类型：

  - 前置通知 2）后置通知 3）环绕通知 4）异常通知 5）最终通知

- 切面（织入）：把通知应用到切入点过程

### AOP切入点表达式

execution：定义通知的切入点。

execution(【注解】 【权限修饰符】【 方法返回值类型 】【操作类型. 方法名称 .（ 参数） 】 【异常】)

- 注解：可选
- 权限修饰符：可选
- 方法返回值类型：必填，可以使用*匹配所有返回值类型
- 操作类型：必填，定义方法所在类的名称，可用*匹配所有类型
- 方法名称：必填，可用*匹配所有方法
- 参数：必填，有5种设计模式
- - ()：没有参数
  - (..)：匹配所有参数
  - (..，java.lang.String)：以String作为最后一个参数，前面参数个数任意
  - (java.lang.String，..)：以String作为第一个参数，后面参数个数任意
  - (*.java.lang.String)：以String作为最后一个参数，前面可以设置任意一个参数
- 异常：可选，可以设置多个，用逗号分割

### 注解方式

Kid类

```java
package com.x17.aop;
import org.springframework.stereotype.Component;
//被增强的类
@Component
public class Kid {
    public void now() {
        System.out.println("now");
    }
}
```

KidPlus类

```java
package com.x17.aop;

import org.aspectj.lang.ProceedingJoinPoint;
import org.aspectj.lang.annotation.*;
import org.springframework.stereotype.Component;

@Component
@Aspect//生成代理对象
public class KidPlus {
    //前置通知
    @Before(value = "execution(* com.x17.aop.Kid.now(..))")
    public void before() {
        System.out.println("before");
    }
    //后置通知（返回通知）
    @AfterReturning(value = "execution(* com.x17.aop.Kid.now(..))")
    public void afterReturning() {
        System.out.println("afterReturning.........");
    }
    //最终通知
    @After(value = "execution(* com.x17.aop.Kid.now(..))")
    public void after() {
        System.out.println("after.........");
    }
    //异常通知
    @AfterThrowing(value = "execution(* com.x17.aop.Kid.now(..))")
    public void afterThrowing() {
        System.out.println("afterThrowing.........");
    }
    //环绕通知
    @Around(value = "execution(* com.x17.aop.Kid.now(..))")
    public void around(ProceedingJoinPoint proceedingJoinPoint) throws Throwable {
        System.out.println("环绕之前.........");
        //被增强的方法执行
        proceedingJoinPoint.proceed();
        System.out.println("环绕之后.........");
    }

}
```

可以用xml或者配置类开启注解扫描和AOP扫描

bean.xml

```xml
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
       xmlns:context="http://www.springframework.org/schema/context"
       xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" xmlns:aop="http://www.springframework.org/schema/aop"
       xsi:schemaLocation="http://www.springframework.org/schema/beans http://www.springframework.org/schema/beans/spring-beans.xsd
       http://www.springframework.org/schema/context http://www.springframework.org/schema/context/spring-context.xsd http://www.springframework.org/schema/aop https://www.springframework.org/schema/aop/spring-aop.xsd">
    <context:component-scan base-package="com.x17"/><!--开启注解扫描-->
    <aop:aspectj-autoproxy/><!--开启AOP扫描-->
</beans>
```

配置类

```java
package com.x17.config;

import org.springframework.context.annotation.ComponentScan;
import org.springframework.context.annotation.Configuration;
import org.springframework.context.annotation.EnableAspectJAutoProxy;

@Configuration //作为配置类，替代 xml 配置文件
@ComponentScan(basePackages = {"com.x17"})
@EnableAspectJAutoProxy//开启AspectJ
public class SpringConfig {
}
```

测试

```java
@Test
public void kid() {
    AnnotationConfigApplicationContext context = new AnnotationConfigApplicationContext(SpringConfig.class);
    Kid kid = context.getBean("kid", Kid.class);
    kid.now();
}
@Test
public void kid1() {
    ApplicationContext context = new ClassPathXmlApplicationContext("bean.xml");
    Kid kid = context.getBean("kid", Kid.class);
    kid.now();
}
```

输出：环绕之前.........
before
now
afterReturning.........
after.........
环绕之后.........

后三个先后顺序和版本有关

#### 抽取相同切入点

上面每个execution都有相同的切入点，可以抽取出来

```java 
//相同切入点抽取
@Pointcut(value = "execution(* com.x17.aop.Kid.now(..))")
public void pointdemo() {}

//前置通知
//@Before注解表示作为前置通知
@Before(value = "pointdemo()")//相同切入点抽取使用
public void before() {
    System.out.println("before.........");
}
```

#### 增强类优先级

当有多个增强类对一个方法做增强时，可使用@Order（数值）设置优先级，决定谁先后执行。

@Order（数值）中数值从0开始，数字越小，优先级越高。

### xml方式

```xml
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
       xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" xmlns:aop="http://www.springframework.org/schema/aop"
       xsi:schemaLocation="http://www.springframework.org/schema/beans http://www.springframework.org/schema/beans/spring-beans.xsd
       http://www.springframework.org/schema/aop https://www.springframework.org/schema/aop/spring-aop.xsd">
    <!--创建对象-->
    <bean id="kid" class="com.x17.aop.Kid"></bean>
    <bean id="kidPlus" class="com.x17.aop.KidPlus"></bean>
    <!--3、在 spring 配置文件中配置切入点-->
    <!--配置 aop 增强-->
    <aop:config>
        <!--切入点-->
        <aop:pointcut id="p" expression="execution(* com.x17.aop.Kid.now(..))"/>
        <!--配置切面-->
        <aop:aspect ref="kidPlus">
            <!--增强作用在具体的方法上-->
            <aop:before method="before" pointcut-ref="p"/>
        </aop:aspect>
    </aop:config>
</beans>
```

## JDBC操作模板

JDBC Template 是Spring提供的一个简单到极致的JDBC操作模板组件，利用该组件可有效解决一些重复设计问题。依托Spring框架的IOC和AOP的操作特征，又可以实现连接配置以及事务处理控制。

### **JDBC Template 和传统 JDBC对比**

|          | 传统 JDBC开发                                                | JDBC Template                                                |
| -------- | :----------------------------------------------------------- | ------------------------------------------------------------ |
| 开发步骤 | ①进行数据库驱动程序的加载。②取得数据库的连接对象。③声明要操作的SQL语句。④创建数据库操作对象。⑤执行SQL语句。⑥处理返回的结果（ResultSet）。⑦关闭结果集对象。⑧关闭数据库操作对象（Statement）。⑨如果执行的是更新应该进行事务的提交或回滚。⑩关闭数据库连接 | ①取得数据库连接对象。②声明要操作的SQL语句。③执行SQL语句。④处理返回的操作结果（ResuleSet） |
| 优点     | ①具备固定的操作流程，代码结构简单。②JDBC是Java的公共服务，属于标准。③由于没有涉及过于复杂的对象操作，所以性能是最高的 | ①代码简单，但又不脱离JDBC形式。②由于有Spring AOP的支持，用户只关心核心。③对于出现的异常可以采用统一的方式进行处理。④与JDBC的操作步骤或形式近乎雷同。 |
| 缺点     | ①代码的冗余度太高。②用户需要手工进行事务的处理操作。③所有的操作必须严格按照既定的步骤执行。④如果出现执行异常，需要自己处理 | ①与重度包装的 ORMapping框架不同，不够智能。②处理返回结果的时候不能自动转化为VO类对象，需要手动处理结果集 |

### **JdbcTemplate常用的操作方法**

| 方法名称                                                     | 描述                                          |
| ------------------------------------------------------------ | --------------------------------------------- |
| public JdbcTemplate()                                        | 定义JDBC Template对象，可以在Spring配置中使用 |
| public JdbcTemplate(DataSource dataSource)                   | 接收DataSource对象                            |
| public <T> List<T> query(String sql, Object[] args, int[] argTypes, RowMapper<T> rowMapper) throws DataAccessException | 执行数据库查询操作                            |
| public <T> T queryForObject(String sql, Class<T> requiredType, @Nullable Object... args) throws DataAccessException | 返回单个查询对象                              |
| public int update(String sql, @Nullable Object... args) throws DataAccessException | 执行更新处理                                  |
| public <T> int[][] batchUpdate(String sql, final Collection<T> batchArgs, final int batchSize, final ParameterizedPreparedStatementSetter<T> pss) throws DataAccessException | 数据批量更新                                  |
| public <T> T queryForObject(String sql, @Nullable Object[] args, RowMapper<T> rowMapper) throws DataAccessException | RowMapper查询                                 |

在 spring 配置文件配置数据库连接池，配置 JdbcTemplate 对象，注入 DataSource

```xml
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
       xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
       xmlns:context="http://www.springframework.org/schema/context"
       xsi:schemaLocation="http://www.springframework.org/schema/beans http://www.springframework.org/schema/beans/spring-beans.xsd
                          http://www.springframework.org/schema/context http://www.springframework.org/schema/context/spring-context.xsd">
    <context:component-scan base-package="com.x17"/>
    <!--配置数据库连接池-->
    <bean id="dataSource" class="com.alibaba.druid.pool.DruidDataSource"
          destroy-method="close">
        <property name="url" value="jdbc:mysql:///hello"/>
        <property name="username" value="root"/>
        <property name="password" value="20020112xing"/>
        <property name="driverClassName" value="com.mysql.jdbc.Driver"/>
    </bean>
    <!-- JdbcTemplate 对象 -->
    <bean id="jdbcTemplate" class="org.springframework.jdbc.core.JdbcTemplate">
        <!--注入 dataSource-->
        <property name="dataSource" ref="dataSource"/><!--set方式注入-->
    </bean>
</beans>
```

### Jdbc Template 实现CRUD操作

数据库

```SQL
CREATE TABLE book 
( 
    id int PRIMARY KEY AUTO_INCREMENT,
    name VARCHAR(20) ,
    price int 
)
```

Book类

```java
package com.x17.pojo;

public class Book {
    private String name;
    private Integer price;
    private Integer id;
    构造器、getter、setter、toString略
}    
```

BookDao接口

```java
package com.x17.dao;

import com.x17.pojo.Book;
import org.springframework.stereotype.Service;
import java.util.List;

@Service
public interface BookDao {
    //添加一条记录
    public int addOne(Book book);
    //批量添加
    public int[] batchAdd(List<Object[]> batchArgs);
    //更新一条记录
    public int updateOne(Book book);
    //批量更新
    public int[] batchUpdate(List<Object[]> batchArgs);
    //删除一条记录
    public int deleteOne(int id);
    //批量删除
    public void batchDelete(List<Object[]> batchArgs);
    //查询单个值
    public int queryOneValue(int id);
    //查询一条记录
    public Book queryOneObj(int id);
    //查询多条记录
    public List<Book> queryListObj();
}
```

BookDaoImpl实现类

```java
package com.x17.dao;

import com.x17.pojo.Book;
import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.jdbc.core.BeanPropertyRowMapper;
import org.springframework.jdbc.core.JdbcTemplate;
import org.springframework.stereotype.Repository;
import java.util.List;

@Repository
public class BookDaoImpl implements BookDao {
    @Autowired
    private JdbcTemplate jdbcTemplate;

    @Override
    public int addOne(Book book) {
        String sql = "insert into book(name, price) values (?,?)";
        return jdbcTemplate.update(sql, book.getName(), book.getPrice());

    }

    @Override
    public int[] batchAdd(List<Object[]> batchArgs) {
        String sql = "insert into book(name, price) values (?,?)";
        return jdbcTemplate.batchUpdate(sql, batchArgs);
    }

    @Override
    public int updateOne(Book book) {
        String sql = "update book set name=?,price=? where id=?";
        return jdbcTemplate.update(sql, book.getName(), book.getPrice(), book.getId());

    }

    @Override
    public int[] batchUpdate(List<Object[]> batchArgs) {
        String sql = "update book set name=?,price=? where id=?";
        return jdbcTemplate.batchUpdate(sql, batchArgs);
    }

    @Override
    public int deleteOne(int id) {
        String sql = "delete from book where id=?";
        return jdbcTemplate.update(sql, id);
    }

    @Override
    public void batchDelete(List<Object[]> batchArgs) {
        String sql = "delete from book where id=?";
        jdbcTemplate.batchUpdate(sql, batchArgs);
    }

    @Override
    public int queryOneValue(int id) {
        String sql = "select count(*) from book";
        return jdbcTemplate.queryForObject(sql, Integer.class);
    }

    @Override
    public Book queryOneObj(int id) {
        String sql = "select * from book where id =?";
        return jdbcTemplate.queryForObject(sql, new BeanPropertyRowMapper<Book>(Book.class), id);
    }

    @Override
    public List<Book> queryListObj() {
        String sql = "select * from book";
        return jdbcTemplate.query(sql, new BeanPropertyRowMapper<Book>(Book.class));
    }
}
```

测试类

```java
@Test
public void addTest() {
    ApplicationContext context = new ClassPathXmlApplicationContext("bean.xml");
    BookDaoImpl bookDaoImpl = context.getBean("bookDaoImpl", BookDaoImpl.class);
    //添加一条记录
    bookDaoImpl.addOne(new Book("java", 30));
    List<Object[]> batchArgs = new ArrayList<>();
    Object[] o1 = {"Linux", 39};
    Object[] o2 = {"数据库", 39};
    batchArgs.add(o1);
    batchArgs.add(o2);
    System.out.println(bookDaoImpl.batchAdd(batchArgs));
    //更新一条记录
    bookDaoImpl.updateOne(new Book("java", 56, 1));
    //更新多条记录
    List<Object[]> batchArgs1 = new ArrayList<>();
    Object[] o3 = {"Linux", 67, 2};
    Object[] o4 = {"数据库", 45, 3};
    batchArgs1.add(o3);
    batchArgs1.add(o4);
    System.out.println(bookDaoImpl.batchUpdate(batchArgs1));
    //查询单值
    System.out.println(bookDaoImpl.queryOneValue(1));
    //查询一条记录
    System.out.println(bookDaoImpl.queryOneObj(1));
    //查询多条记录
    System.out.println(bookDaoImpl.queryListObj());
    //删除一条记录
    bookDaoImpl.deleteOne(1);
    //删除多条记录
    List<Object[]> batchArgs2 = new ArrayList<>();
    Object[] o5 = {2};
    Object[] o6 = {3};
    batchArgs2.add(o5);
    batchArgs2.add(o6);
    bookDaoImpl.batchDelete(batchArgs2);
}
```

## Spring事务管理

### 回顾传统JDBC及事务

事务控制的核心是对数据库中数据操作的完整性保证。

#### 事务的ACID原则

- 原子性（Atomicity）：一个事务的所有操作，要么全部提交，要么全部回滚。
- 一致性（Consistency）：事务必须始终保持系统处于一致的状态，不管并发事务有多少。
- 隔离性（Isolation）：事务之间互相隔离，一个事务不影响另一个事务的操作。
- 持久性（Durability）：事务完成后，该事务对数据库所进行的更改将持久保存在数据库中。

#### 传统JDBC事务管理

| 方法                               | 描述               |
| ---------------------------------- | ------------------ |
| setAutoCommit（boolean autoCommit) | 取消自动的事务提交 |
| commit（）                         | 手动事务提交       |
| rollback（）                       | 手动事务回滚       |
| setSavepoint（String name）        | 设置事务保存点     |

### 事务传播属性

事务传播属性指的是不同层之间进行的事务控制处理。传播属性在TransactionDefinition接口中定义。

| 传播属性      | 描述                                                         |
| ------------- | ------------------------------------------------------------ |
| REQUIRED      | 如果存在事务，则支持当前事务。如果没有事务，则开启新的事务。（默认） |
| SUPPORTS      | 如果存在事务，则支持当前事务。如果没有事务，则非事务的执行。 |
| MANDATORY     | 如果存在事务，则支持当前事务。如果没有活动事务，则抛出异常。 |
| REQUIRES_NEW  | 总是开启新的事务。如果已经存在事务，则将已存在的事务挂起。   |
| NOT_SUPPORTED | 总是非事务的执行，并挂起任何存在的活动事务。                 |
| NEVER         | 总是非事务的执行。如果存在活动事务，则抛出异常。             |
| NESTED        | 如果存在活动事务，则将其运行在一个嵌套事务中。如果没有事务，则按REQUIRED属性执行。 |

###   事务隔离级别

| 事务隔离级别       | 描述                                                         |
| ------------------ | ------------------------------------------------------------ |
| DEFAULT            | 默认的隔离级别，使用数据库默认的隔离级别                     |
| READ_UNCOMOMMITTED | 最低的隔离级别，允许其他事务访问当前事务未提交的数据。会产生脏读、不可重读、幻读。 |
| READ_COMOMMITTED   | 当前事务修改的数据提交后才能被下一个事务读取。下一个事务不能读取当前事务未提交的数据。可防止脏读，可能出现不可重复读。幻读。 |
| REPEATABLE_READ    | 可防止脏读、不可重复读，可能出现幻读                         |
| SERIALIZABLE       | 最可靠的事务隔离级别，事务处理为顺序执行                     |

### 注解实现声明式事务管理

使用**@Transaction**注解配置

@Transaction属性

| 属性                 | 描述                                                         |
| -------------------- | ------------------------------------------------------------ |
| propagation          | 设置事务的传播属性                                           |
| isolation            | 设置事务的隔离级别                                           |
| timeout              | 设置事务超时时间，如果为-1，则表示永不超时                   |
| readOnly             | 设置为只读事务，设置为true表示只读，设置为false表示读写      |
| rollbackFor          | 设置回滚的异常类型，可设置多个类型。单一异常：@Transaction（rollbackFor=RuntimeException.class)         多个异常：@Transaction（rollbackFor=RuntimeException.class，Exception.class) |
| rollbackForClassName | 设置回滚异常类名称。单一异常：@Transaction（rollbackForClassName=“RuntimeException”)         多个异常：@Transaction（rollbackForClassName={"RuntimeException"，"Exception") |
| noRollbackFor        | 设置不回滚的异常类型。单一异常：@Transaction（noRollbackFor=RuntimeException.class)         多个异常：@Transaction（noRollbackFor=RuntimeException.class，Exception.class) |
| rollbackForClassName | 设置不回滚异常类名称。单一异常：@Transaction（noRollbackForClassName=“RuntimeException”)         多个异常：@Transaction（noRollbackForClassName={"RuntimeException"，"Exception") |

bean.xml

```xml
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
       xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
       xmlns:context="http://www.springframework.org/schema/context"
       xmlns:tx="http://www.springframework.org/schema/tx"
       xsi:schemaLocation="http://www.springframework.org/schema/beans http://www.springframework.org/schema/beans/spring-beans.xsd
                          http://www.springframework.org/schema/context http://www.springframework.org/schema/context/spring-context.xsd
                          http://www.springframework.org/schema/tx http://www.springframework.org/schema/tx/spring-tx.xsd
                          ">
    <context:component-scan base-package="com.x17"/>
    <!--配置数据库连接池-->
    <bean id="dataSource" class="com.alibaba.druid.pool.DruidDataSource"
          destroy-method="close">
        <property name="url" value="jdbc:mysql:///hello"/>
        <property name="username" value="root"/>
        <property name="password" value="20020112xing"/>
        <property name="driverClassName" value="com.mysql.cj.jdbc.Driver"/>
    </bean>
    <!-- JdbcTemplate 对象 -->
    <bean id="jdbcTemplate" class="org.springframework.jdbc.core.JdbcTemplate">
        <!--注入 dataSource-->
        <property name="dataSource" ref="dataSource"/><!--set方式注入-->
    </bean>
    <!--创建事务管理器-->
    <bean id="transactionManager" class="org.springframework.jdbc.datasource.DataSourceTransactionManager">
        <property name="dataSource" ref="dataSource"/>
    </bean>

    <!--开启事务注解-->
    <tx:annotation-driven transaction-manager="transactionManager"/>

</beans>
```

为了方便起见，使用之前的bookDao做一个简单的例子。

```java
package com.x17.service;

import com.x17.dao.BookDao;
import com.x17.pojo.Book;
import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.stereotype.Component;
import org.springframework.transaction.annotation.Isolation;
import org.springframework.transaction.annotation.Propagation;
import org.springframework.transaction.annotation.Transactional;

@Component
@Transactional(propagation = Propagation.REQUIRED,isolation = Isolation.SERIALIZABLE)
public class BookService {
    @Autowired
    private BookDao bookDao;

    public void test(){
        bookDao.updateOne(new Book("java",34,1));
        int a=100/0;//模拟异常
        bookDao.updateOne(new Book("java",34,1));
    }
}
```

使用@Transaction注解之后，当出现异常就会回滚，保证数据完整性。

#### 完全注解开发

使用配置类，去除XML。

```java
package com.x17.config;

import com.alibaba.druid.pool.DruidDataSource;
import org.springframework.context.annotation.Bean;
import org.springframework.context.annotation.ComponentScan;
import org.springframework.context.annotation.Configuration;
import org.springframework.context.annotation.EnableAspectJAutoProxy;
import org.springframework.jdbc.core.JdbcTemplate;
import org.springframework.jdbc.datasource.DataSourceTransactionManager;
import org.springframework.transaction.annotation.EnableTransactionManagement;
import javax.sql.DataSource;

@Configuration //作为配置类，替代 xml 配置文件
@ComponentScan(basePackages = {"com.x17"})
@EnableAspectJAutoProxy//开启AspectJ
@EnableTransactionManagement  //开启事务
public class SpringConfig {

    //创建数据库连接池
    @Bean
    public DruidDataSource getDruidDataSource() {
        DruidDataSource druidDataSource = new DruidDataSource();
        druidDataSource.setDriverClassName("com.mysql.cj.jdbc.Driver");
        druidDataSource.setUrl("jdbc:mysql://localhost:3306/hello");
        druidDataSource.setUsername("root");
        druidDataSource.setPassword("20020112xing");
        return druidDataSource;
    }

    //创建JdbcTemplate对象
    @Bean
    public JdbcTemplate getJdbcTemplate(DataSource dataSource) {
        JdbcTemplate jdbcTemplate = new JdbcTemplate();
        jdbcTemplate.setDataSource(dataSource);
        return jdbcTemplate;
    }

    //创建事务管理器
    @Bean
    public DataSourceTransactionManager getDataSourceTransactionManager(DataSource dataSource) {
        DataSourceTransactionManager transactionManager = new DataSourceTransactionManager();
        transactionManager.setDataSource(dataSource);
        return transactionManager;
    }
}
```


