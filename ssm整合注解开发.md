# 流程分析

1. 创建工程
   - 创建一个Maven的web工程
   - pom.xml添加SSM需要的依赖jar包
   - 编写Web项目的入口配置类，实现AbstractAnnotationConfigDispatcherServletInitializer重写以下方法
     - `getRootConfigClasses()` ：返回Spring的配置类 —> 需要`SpringConfig`配置类
     - `getServletConfigClasses()` ：返回SpringMVC的配置类 —> 需要`SpringMvcConfig`配置类
     - `getServletMappings()` : 设置SpringMVC请求拦截路径规则
     - `getServletFilters()` ：设置过滤器，解决POST请求中文乱码问题
2. SSM整合(重点是各个配置的编写)
   - SpringConfig
     - 标识该类为配置类，使用`@Configuration`
     - 扫描`Service`所在的包，使用`@ComponentScan`
     - 在`Service`层要管理事务，使用`@EnableTransactionManagement`
     - 读取外部的`properties`配置文件，使用`@PropertySource`
     - 整合Mybatis需要引入Mybatis相关配置类，使用@Import
       - 第三方数据源配置类 `JdbcConfig`
       - 构建DataSource数据源，DruidDataSouroce，需要注入数据库连接四要素，使用`@Bean`、`@Value`
       - 构建平台事务管理器，DataSourceTransactionManager，使用`@Bean`
       - Mybatis配置类 `MybatisConfig`
       - 构建`SqlSessionFactoryBean`并设置别名扫描与数据源，使用`@Bean`
       - 构建`MapperScannerConfigurer`并设置DAO层的包扫描
   - SpringMvcConfig
     - 标识该类为配置类，使用`@Configuratio`
     - 扫描`Controller`所在的包，使用`@ComponentScan`
     - 开启SpringMVC注解支持，使用`@EnableWebMvc`
3. 功能模块(与具体的业务模块有关)
   - 创建数据库表
   - 根据数据库表创建对应的模型类
   - 通过Dao层完成数据库表的增删改查(接口+自动代理)
   - 编写Service层(Service接口+实现类)
     - `@Service`
     - `@Transactional`
     - 整合Junit对业务层进行单元测试
       - `@RunWith`
       - `@ContextConfiguration`
       - `@Test`
   - 编写Controller层
     - 接收请求 `@RequestMapping`、`@GetMapping`、`@PostMapping`、`@PutMapping`、`@DeleteMapping`
     - 接收数据 简单、POJO、嵌套POJO、集合、数组、JSON数据类型
       - `@RequestParam`
       - `@PathVariable`
       - `@RequestBody`
     - 转发业务层
       - `@Autowired`
     - 响应结果
       - `@ResponseBody`

# 整合配置

分析完毕之后，我们就一步步来完成我们的SSM整合

- 步骤一：创建Maven的web项目

- 步骤二：

  导入坐标

  ```xml
  <dependencies>
      <dependency>
          <groupId>org.springframework</groupId>
          <artifactId>spring-webmvc</artifactId>
          <version>5.2.10.RELEASE</version>
      </dependency>
  
      <dependency>
          <groupId>org.springframework</groupId>
          <artifactId>spring-jdbc</artifactId>
          <version>5.2.10.RELEASE</version>
      </dependency>
  
      <dependency>
          <groupId>org.springframework</groupId>
          <artifactId>spring-test</artifactId>
          <version>5.2.10.RELEASE</version>
      </dependency>
  
      <dependency>
          <groupId>org.mybatis</groupId>
          <artifactId>mybatis</artifactId>
          <version>3.5.6</version>
      </dependency>
  
      <dependency>
          <groupId>org.mybatis</groupId>
          <artifactId>mybatis-spring</artifactId>
          <version>1.3.0</version>
      </dependency>
  
      <dependency>
          <groupId>mysql</groupId>
          <artifactId>mysql-connector-java</artifactId>
          <version>5.1.46</version>
      </dependency>
  
      <dependency>
          <groupId>com.alibaba</groupId>
          <artifactId>druid</artifactId>
          <version>1.1.16</version>
      </dependency>
  
      <dependency>
          <groupId>junit</groupId>
          <artifactId>junit</artifactId>
          <version>4.12</version>
          <scope>test</scope>
      </dependency>
  
      <dependency>
          <groupId>javax.servlet</groupId>
          <artifactId>javax.servlet-api</artifactId>
          <version>3.1.0</version>
          <scope>provided</scope>
      </dependency>
  
      <dependency>
          <groupId>com.fasterxml.jackson.core</groupId>
          <artifactId>jackson-databind</artifactId>
          <version>2.9.0</version>
      </dependency>
  </dependencies>
  ```

- 步骤三：创建项目包结构

  - `com.blog.config`目录存放的是相关的配置类
  - `com.blog.controller`编写的是Controller类
  - `com.blog.dao`存放的是Dao接口，因为使用的是Mapper接口代理方式，所以没有实现类包
  - `com.blog.service`存的是Service接口，`com.blog.service.impl`存放的是Service实现类
  - `resources`:存入的是配置文件，如Jdbc.properties
  - `webapp`:目录可以存放静态资源
  - `test/java`:存放的是测试类

- 步骤四：创建jdbc.properties

  ```PROPERTIES
  jdbc.driver=com.mysql.jdbc.Driver
  jdbc.url=jdbc:mysql://localhost:13306/ssm_db?useSSL=false
  jdbc.username=root
  jdbc.password=PASSWORD.
  ```

- 步骤五：

  创建JdbcConfig配置类

  ```JAVA
  public class JdbcConfig {
      @Value("${jdbc.driver}")
      private String driver;
      @Value("${jdbc.url}")
      private String url;
      @Value("${jdbc.username}")
      private String username;
      @Value("${jdbc.password}")
      private String password;
  	
      //创建数据源
      @Bean
      public DataSource dataSource() {
          DruidDataSource dataSource = new DruidDataSource();
          dataSource.setDriverClassName(driver);
          dataSource.setUrl(url);
          dataSource.setUsername(username);
          dataSource.setPassword(password);
          return dataSource;
      }
  	//构建平台事务管理器
      @Bean
      public PlatformTransactionManager platformTransactionManager(DataSource dataSource){
          DataSourceTransactionManager ds = new DataSourceTransactionManager();
          ds.setDataSource(dataSource);//设置数据源
          return ds;
      }
  }
  ```

- 步骤六：

  创建MyBatisConfig配置类

  ```JAVA
  public class MyBatisConfig {
      @Bean
      public SqlSessionFactoryBean sqlSessionFactory(DataSource dataSource){
          SqlSessionFactoryBean factoryBean = new SqlSessionFactoryBean();
          factoryBean.setDataSource(dataSource);
          factoryBean.setTypeAliasesPackage("com.jason.pojo");
          return factoryBean;
      }
  
      @Bean
      public MapperScannerConfigurer mapperScannerConfigurer(){
          MapperScannerConfigurer msc = new MapperScannerConfigurer();
          msc.setBasePackage("com.jason.dao");
          return msc;
      }
  }
  ```

- 步骤七：

  创建SpringConfig配置类

  ```JAVA
  @Configuration
  @ComponentScan("com.blog.service")
  @PropertySource("jdbc.properties")
  @EnableTransactionManagement
  @Import({JdbcConfig.class, MyBatisConfig.class})
  public class SpringConfig {
  }
  ```

- 步骤八：

  创建SpringMvcConfig配置类

  ```JAVA
  @Configuration
  @ComponentScan("com.jason.controller")
  @EnableWebMvc
  public class SpringMvcConfig {
  }
  ```

- 步骤九：

  创建ServletContainersInitConfig配置类

  ```JAVA
  public class ServletContainersInitConfig extends AbstractAnnotationConfigDispatcherServletInitializer {
      protected Class<?>[] getRootConfigClasses() {
          return new Class[]{SpringConfig.class};
      }
  
      protected Class<?>[] getServletConfigClasses() {
          return new Class[]{SpringMvcConfig.class};
      }
  
      protected String[] getServletMappings() {
          return new String[]{"/"};
      }
  
      @Override
      protected Filter[] getServletFilters() {
          CharacterEncodingFilter filter = new CharacterEncodingFilter();
          filter.setEncoding("utf-8");
          return new Filter[]{filter};
      }
  }
  ```