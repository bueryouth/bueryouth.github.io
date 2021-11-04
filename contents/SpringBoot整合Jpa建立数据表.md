
@[TOC](Spring Boot整合Jpa建立数据表)
## JPA简介
- **JPA**由EJB 3.0软件专家组开发，作为JSR-220实现的一部分。但它又不限于EJB 3.0，你可以在Web应用、甚至桌面应用中使用。JPA的宗旨是为POJO提供持久化标准规范，由此可见，经过这几年的实践探索，能够脱离容器独立运行，方便开发和测试的理念已经深入人心了。Hibernate3.2+、TopLink 10.1.3以及OpenJPA都提供了JPA的实现。
[关于Spring Data JPA](https://spring.io/projects/spring-data-jpa)



## 场景介绍

- 现有配置完好的MySQL数据库环境，按照已知的MySQL语句，建立实体类并使用Jpa实现快速建立数据表。

	```sql
	CREATE TABLE `test`.`admin`  (
	  `id` int(11) NOT NULL AUTO_INCREMENT,
	  `name` varchar(88) NULL,
	  `age` int(11) NULL,
	  PRIMARY KEY (`id`)
	)
	```



## 实例环境及工具

- IDEA开发工具，MySQL数据库， Spring Boot框架，Maven项目管理工具



## 引入依赖启动器

1. 在建好Spring Boot项目的前提下，向**pom.xml**添加Spring Data JPA依赖启动器。
	```xml
	<dependency>
	    <groupId>org.springframework.boot</groupId>
	    <artifactId>spring-boot-starter-data-jpa</artifactId>
	</dependency>
	```
2. 向**pom.xml**添加MySQL驱动依赖启动器。
	```xml
	<dependency>
	    <groupId>mysql</groupId>
	    <artifactId>mysql-connector-java</artifactId>
	    <scope>runtime</scope>
	</dependency>
	```



## 全局配置连接MySQL

- 以.yml为例，若是.properties文件，则应需更改
	```yml
	spring:
	  datasource:
	    # 连接数据库
	    url: jdbc:mysql://localhost:3306/test?serverTimezone=UTC
	    # 数据库驱动
	    driver-class-name: com.mysql.cj.jdbc.Driver
	    # 数据库用户名
	    username: root
	    # 数据库密码
	    password: root
	  jpa:
	    # 是否开启缓存
	    show-sql: false
	    # 数据库类型
	    database: MYSQL
	    # 对数据库操作
	    hibernate:
	      ddl-auto: update
	```
- spring.jpa.hibernate.ddl-auto四个属性的用法

	|关键词|用法|
	|:--|:--|
	|create|启动时按照实体类删数据库中之前的表，然后创建新表，退出时不删除该表|
	|create-drop|启动时按照实体类删数据库中之前的表，然后创建新表，退出时删除数据表，如果表不存在报错|
	|update|（常用）第一次启动根据实体建立表结构，如果实体和数据表格式不一致则更新表，原有数据保留|
	|validate|启动时会验证创建数据库表结构，如果不一致则报错|



## 编写测试

1. 在**entity**层添加**AdminEntity.java类**

  ![在这里插入图片描述](https://img-blog.csdnimg.cn/20210629095304136.png)

2. 编写实体类AdminEntity.java
    ```java
    /**
    * @author BuerYouth
    */
    @Entity(name = "admin")
    public class AdminEntity {
        @Id
        @Column(length = 11)
        @GeneratedValue(strategy = GenerationType.IDENTITY)
        private Integer id;
        @Column(length = 88)
        private String name;
        @Column(length = 11)
        private Integer age;
        /**
        * 忽略设置Getter和Setter方法
        */
    }
    ```

3. 在**repository**层编写**AdminRepository.java类**
  · AdminRepository继承JpaRepository接口

   ```java
   public interface AdminRepository extends JpaRepository<AdminEntity, Integer> {}
   ```

4. 在**XXXApplicationTests.java**测试类中编写测试方法
  ```java
  @SpringBootTest
  class MySQLApplicationTests {
      @Autowired
      private AdminRepository adminRepository;
  
      @Test
      public void CreateTable() {
          //使用查询语句做测试
          adminRepository.findAll();
      }
  }
  ```



## 运行测试

1. 开启MySQL服务
2. 运行IDEA中的测试方法
  ![在这里插入图片描述](https://img-blog.csdnimg.cn/20210629095657157.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzQzNjU2Mzk3,size_16,color_FFFFFF,t_70)

3. 打开命令行查看表结构
  ```sql
  mysql> desc admin;
  ```
  ![在这里插入图片描述](https://img-blog.csdnimg.cn/2021062910020457.png)



## 其他

- Jpa部分注解
	
	|**注解**|**含义**|
	|:---|:--|
	|@Entity|标识实体类是JPA实体，告诉JPA在程序运行时生成实体类对应表|
	|@Table|设置实体类在数据库所对应的表名|
	|@Id|标识类里所在变量为主键|
	|@GeneratedValue|设置主键生成策略，此方式依赖于具体的数据库|
	|@Column|表示属性所对应字段名进行个性化设置|
	|@Basic|表示简单属性到数据库表字段的映射|
	|@Transient|表示属性并非数据库表字段的映射,ORM框架将忽略该属性|
	|@Temporal|当我们使用到java.util包中的时间日期类型，则需要此注释来说明转化成java.util包中的类型。|
	|@Enumerated|使用此注解映射枚举字段，以String类型存入数据库|
	|@Embeddable|注解在类上，表示此类是可以被其他类嵌套|
	|@Embedded|注解在属性上，表示嵌套被@Embeddable注解的同类型类|
	|@MappedSuperclass|实现将实体类的多个属性分别封装到不同的非实体类中|
